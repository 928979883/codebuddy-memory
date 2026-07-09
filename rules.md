# 用户全局编码规则（User Rules）

适用所有项目。CodeBuddy 在每次对话自动加载这些约定。

> ⚠️ 重要：用户有**多套不同代技术栈**，务必先确认当前项目是哪一个，再套用对应规范。以下按"项目族"区分。

---

## 项目地图（先对号入座）
| 项目 | 路径 | 栈 |
|------|------|-----|
| 官网前台 | `E:/德扬能源官网/src/` | Vue 2 + Vue CLI + vue-router 3，**无 UI 库** |
| 老 admin（旺远SaaS） | `E:/徳杨能源/src/main/ui/` | Vue 2 + Vue CLI + Vuex + Element UI |
| **新 admin（官网后台）** | `E:/德扬能源官网/admin/` | **Vue 3 + Vite 8 + Pinia 3 + Element Plus 2 + Vue Router 5 + UnoCSS + vxe-table** |

- 新项目优先采用新 admin 的技术栈（Vue 3 / Vite / Pinia / Element Plus）。
- 基本不使用 React + TypeScript。

---

## 新 admin（官网后台）规范 —— 最优先参考基线
路径：`E:/德扬能源官网/admin/`

### 技术栈
- Vue 3.5（`<script setup>` + Composition API）、Vite 8、Pinia 3、Element Plus 2.13.5、Vue Router 5、UnoCSS、vxe-table、@wangeditor-next。
- 提交用 **commitizen / git-cz**（可选中文提交信息）。

### 样式
- 组件样式 `<style lang="scss" scoped">`。
- 同时启用 **UnoCSS**（原子化 class 可直接用）。

### 代码格式化（Prettier + ESLint flat config，以此为准）
- ⚠️ 与老项目**相反**：**使用双引号、使用分号**（`singleQuote: false`、`semi: true`）。
- 2 空格缩进（`tabWidth: 2`）、`printWidth: 100`、`trailingComma: es5`、`arrowParens: always`。
- ESLint（`eslint.config.mjs`）关键：
  - `eqeqeq: off`、`no-var: error`、`prefer-const: error`、`object-shorthand: error`。
  - `no-unused-vars: off`、`no-undef: off`（依赖 auto-import + Element Plus 全局组件，不报未定义）。
  - Vue 规则：`block-order` 为 `template → script → style`；组件名 PascalCase；`vue/no-v-html: off`；`require-explicit-emits: error`。
  - 允许 `no-console`（仅 production 为 warn）。

### Pinia 结构
- `src/stores/index.js`：`createPinia()` + `setupStore(app)` + 统一 `export * from "./xxx"`。
- 子 store 在 `src/stores/*.js`（如 user.js、dict.js、permission.js、app.js、settings.js、tags-view.js、tenant.js）。
- 命名：`useUserStore` 用 `defineStore("user", () => {...})` setup 写法；并导出 `useUserStoreHook()` = `useUserStore(store)` 供非组件（如 request.js）调用。
- 跨模块调用：在 store 内 `import { useOtherStoreHook } from "@/stores"` 互相引用。

### Axios 请求封装（公司统一，见项目级规则 detail）
- 统一走 `@/utils/request`（默认导出 `http`）。
- 成功返回 `response.data`；旧后端 `code===1` 为业务异常；`code` 为 undefined 直接返回 data；blob/arraybuffer 返回完整 response。
- 401 自动刷新 token（内置队列，业务代码勿自处理）；424/502 跳登录；403 提示无权限。
- 请求拦截器自动附加：Bearer token、TENANT-ID:1、Platform-Code:"business"、post/put/delete 自动加 businessNo=uuid.v1()。

---

## 老项目（官网前台 / 老 admin）规范
- 官网前台：Vue 2 Options API、vue-router 3、**无 UI 库**。request 极简：`baseURL='/api'`、token 用 `headers.auth`、带 `lang` 头、成功判断 `code==='00000'||0`、**无登录逻辑**。
- 老 admin（旺远）：Vue 2 + Vuex + Element UI，重 request（json-bigint、errorCode、401→FedLogOut/LogOut）。
- 老项目 ESLint（`.eslintrc.js`，vue-cli 旧模板）：**单引号、无分号、2 空格、函数括号前无空格、eqeqeq always、prefer-const、允许 console**。
- 改老项目时**不要**套用新 admin 的双引号/分号/`<script setup>` 写法。

---

## 通用约定
- 提交信息：默认中文（新 admin 也可用 git-cz 生成规范提交）。
- 关键逻辑加中文注释。
- 避免魔法数字/字符串，提取为具名常量。
- 函数尽量短小，优先早期返回。
- 已配置：用户发 "1" 时，自动生成 commit message 并提交暂存区所有文件，再 git push。

---

## 自动记忆协议（跨账号 / 跨电脑不丢失）
- 账号记忆（Memory 工具）绑定登录账号，切换账号/电脑会丢失。**项目根目录的 `PROJECT_MEMORY.md` 是"可携带记忆"**，随 git 持久化，等效于记忆。
- **自动读取**：每次对话开始时，先读取当前项目根目录的 `PROJECT_MEMORY.md`（若不存在则跳过），以恢复项目上下文；再读取本全局规则与 `.codebuddy/rules/`。
- **自动写入**：每处理一个项目学到的新约定/坑/偏好，追加到 `PROJECT_MEMORY.md` 的"项目专属记忆"章节（标注日期），并同步写账号记忆（若当前会话可用）。
- 优先以 `PROJECT_MEMORY.md` + `rules/` 为准，确保换账号、换电脑后仍"认识用户"。
- 详见 `工作区/.codebuddy/README.md`（含 git 跟踪说明）。
