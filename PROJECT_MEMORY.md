# 项目记忆（PROJECT_MEMORY）

> 本文件是项目的**可携带记忆**，放在项目根目录，随 git 跨账号/跨电脑持久化。
> CodeBuddy 在每次对话开始时应先读取本文件（见全局规则"自动读取协议"），等效于"有记忆"。
> 切换账号或电脑后，只要本文件随项目存在，AI 即可恢复全部上下文。
>
> **维护约定**：每处理本项目学到的新约定/坑/偏好，追加到"项目专属记忆"章节，并标注日期。

---

## 一、用户技术栈与代码风格（多项目区分，2026-07-09 确认）

### 1. 新 admin（官网后台）= `E:/德扬能源官网/admin/`
- Vue 3.5 + Vite 8 + Pinia 3 + Element Plus 2.13.5 + Vue Router 5 + UnoCSS + vxe-table + @wangeditor-next。
- 格式化：**Prettier + ESLint flat config（eslint.config.mjs）** → **双引号、使用分号、2 空格、printWidth 100、trailingComma es5、arrowParens always**。
- ESLint 关键：`eqeqeq: off`、`no-var: error`、`prefer-const: error`、`object-shorthand: error`、`no-unused-vars: off`、`no-undef: off`（auto-import）、`no-console` 仅 production warn。
- Vue 规则：`block-order` template→script→style；组件名 PascalCase；`vue/no-v-html: off`；`require-explicit-emits: error`。
- Pinia：`src/stores/index.js` 用 `createPinia()` + `setupStore(app)` + `export * from "./xxx"`；`useXxxStore = defineStore("xxx", () => {...})` setup 写法；并导出 `useXxxStoreHook() { return useXxxStore(store) }` 供非组件调用。
- axios 统一 `@/utils/request`：成功返回 `response.data`；旧后端 `code===1` 业务异常；`code` undefined 返回 data；blob/arraybuffer 返回完整 response；401 自动刷新 token（队列防并发，调 `useUserStoreHook().refreshToken()`）；424/502 跳登录；403 提示无权限。请求拦截器自动加 Bearer token、`TENANT-ID:1`、`Platform-Code:"business"`、`post/put/delete` 自动 `businessNo=uuid.v1()`。
- 提交：`git-cz`（commitizen + cz-git）+ husky + lint-staged；也可中文提交。

### 2. 官网前台 = `E:/德扬能源官网/src/`
- Vue 2 + Vue CLI + vue-router 3，**无 UI 组件库**。
- request 极简：`baseURL='/api'`、token 用 `headers.auth`、带 `lang` 头（CN/EN）、成功判断 `code==='00000'||0`、**无登录逻辑**。
- `utils/auth.js` 用 localStorage：`deyang_auth_token` / `deyang_username`。

### 3. 老 admin（旺远 SaaS）= `E:/徳杨能源/src/main/ui/`
- Vue 2 + Vue CLI + Vuex + Element UI，重 request（json-bigint、errorCode、`store.dispatch('FedLogOut'/'LogOut')`、401 跳登录）。
- 老项目 ESLint（`.eslintrc.js`）：单引号、无分号、2 空格、函数括号前无空格、`eqeqeq` always、`prefer-const`、允许 console。

### 通用
- 样式：新项目 `<style lang="scss" scoped">` + UnoCSS。
- 关键逻辑加中文注释；避免魔法数字；函数短小、早期返回。
- 用户发 "1" → 自动生成 commit message 并提交暂存区所有文件，再 git push。

---

## 二、项目专属记忆（自动追加区）

<!-- 每处理本项目，在此追加：日期、发现、坑、特殊约定 -->

### [2026-07-09] 项目地图澄清
- 真正的新 admin 在 `E:/德扬能源官网/admin/`（不是 `徳杨能源/src/main/ui/`，那个是老旺远 SaaS）。
- 官网前台 `E:/德扬能源官网/src/` 确为 Vue2 无 UI 库、极简 request 无登录。

---

## 三、待办 / 开放问题
- [ ] 是否单独为老旺远 admin 建规则文件？
- [ ] 官网前台是否已引入 Element Plus？（当前判断：未引入）
- [ ] 全局记忆仓库托管平台：GitLab 还是 GitHub？（待用户决定）
