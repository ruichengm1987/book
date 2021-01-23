---
title: bug规则
weight: 1
---

# bug 规则
## 研发BUG处理规范
* BUG状态及时更新，附BUG状态说明
    * New - 新创建，该状态P0/P1级别问题不要超过一天，P2不超过2天
    * Assign - 已分配
    * Duplicate - 重复bug，通过链接把相关bug link在一起
    * Invalid - 无效bug
    * Wontfix - 确认是bug，但是无需修复
    * workaround - 确认是bug，但是可以通过其他方式绕过
    * Worksforme - 无法复现，需要测试提供更多信息
    * Fixed - 已修复
* BUG及时分配给对应人员，同时写明原因，不是自己处理范围的问题不要挂在自己身上
* BUG及时回复，通过文字记录修复过程，避免口口相传
    * Track过程中阶段性结论
    * 与相关人员沟通的信息
* BUG修复，写清楚修复逻辑以及对应build版本号
    * 便于测试理解修复原理
    * 便于后续Review问题