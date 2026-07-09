# CodeBuddy 用户级记忆（跨所有项目自动加载）

> 想让我更懂你，就改这个文件。启动 CodeBuddy 时自动注入上下文。
> ⚠️ 本文件绑账号云端同步，换电脑登录即恢复。

## 启动同步（每次对话必须执行）
每次对话开始，**自动执行以下命令**从 GitHub 同步最新记忆到本地（无需用户提醒）：
```powershell
# 1. 拉取记忆仓库
if (Test-Path "C:/Users/28345/.codebuddy-sync") {
  cd "C:/Users/28345/.codebuddy-sync"; git pull origin main
} else {
  git clone https://github.com/928979883/codebuddy-memory.git "C:/Users/28345/.codebuddy-sync"
}
# 2. 覆盖本地记忆文件
copy "C:/Users/28345/.codebuddy-sync/rules.md" "C:/Users/28345/.codebuddy/" -Force
copy "C:/Users/28345/.codebuddy-sync/settings.json" "C:/Users/28345/.codebuddy/" -Force
copy "C:/Users/28345/.codebuddy-sync/mcp.json" "C:/Users/28345/.codebuddy/" -Force
copy "C:/Users/28345/.codebuddy-sync/models.json" "C:/Users/28345/.codebuddy/" -Force
```
执行后简要报告结果。

## 身份
- 前端开发工程师，base 长沙。
- 多项目并行：德扬能源官网、杂货铺 PWA 收银系统、微信小程序。

## 沟通与协作偏好
- 极简高效，不绕弯；给方案给结果，不灌鸡汤。
- 决策用单字母 / 编号，结构化确认。
- 讨论工具本质或技术问题时直接给结论，不必铺垫基础概念或科普。

## 工程关注点
- 适老化设计、网络加速、磁盘整理。
- 默认前端栈以项目实际为准；框架 / 库选择先给选项再定。

## 协作习惯
- 重复流程提议存成 skill / 自定义指令，下次一句话复用。
- 纠错请明说（对 / 错 + 原因），我会更新记忆并在后续避免。
- 每个新项目先用 /init 生成项目级 CODEBUDDY.md。
