US-1 邮箱登录
Acceptance Criteria
GIVEN 用户输入合法邮箱
WHEN 用户完成邮箱验证
THEN 用户可以成功登录
AND 密码错误时返回明确错误信息
AND 登录状态可在刷新后保持

US-2 多语言支持
Acceptance Criteria
GIVEN 用户切换语言
WHEN 页面刷新
THEN 所有 UI 文案按目标语言显示
AND 不出现混合语言
AND 语言设置持久化保存

US-3 & US-4 多币种 + 汇率
Acceptance Criteria
GIVEN 用户输入非基础币种金额
WHEN 保存账目
THEN 系统记录原始币种与金额
AND 自动按当日汇率换算
AND 汇率来源可配置（API）
AND 换算结果可追溯

US-5 多人共享账本
Acceptance Criteria
GIVEN 用户邀请其他用户
WHEN 对方接受邀请
THEN 双方可看到同一账本
AND 权限（只读/可编辑）可区分
AND 修改实时同步

US-8 定期支出
Acceptance Criteria
GIVEN 用户设置周期性支出
WHEN 到达触发日期
THEN 系统自动生成账目
AND 允许暂停/修改/终止
AND 不重复生成

US-9 / US-10 预算
Acceptance Criteria
GIVEN 已设置预算
WHEN 支出超过阈值
THEN 系统提示超支
AND 分类预算独立计算
AND 月度自动重置（可配置）

US-11 筛选
Acceptance Criteria
GIVEN 多条账目存在
WHEN 用户设置筛选条件
THEN 返回结果完全符合条件
AND 多条件可组合
AND 性能在大数据量下可接受

US-12 图表
Acceptance Criteria
GIVEN 有足够账目数据
WHEN 打开报表页面
THEN 饼图正确反映分类占比
AND 数值与账目一致
AND 时间范围可切换

US-13 AI 分析
Acceptance Criteria
GIVEN 至少一个月数据
WHEN 用户请求分析
THEN 系统生成可读的支出总结
AND 提供至少一条可执行建议
AND 不暴露原始隐私数据

US-14 / US-15 导入导出
Acceptance Criteria
GIVEN 标准格式文件（CSV / JSON）
WHEN 导入
THEN 数据结构正确映射
AND 错误行有提示
AND 导出数据完整可复现账本