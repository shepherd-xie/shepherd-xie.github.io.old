---
title: IDEA中打开Dashboard
date: 2019-01-30 13:30:00
tags: 
categories: IDEA
top:
---

在 `.idea` 文件夹中的 `workspace.xml` 中找到

<!-- more -->

```xml
  <component name="RunDashboard">
    <option name="ruleStates">
      <list>
        <RuleState>
          <option name="name" value="ConfigurationTypeDashboardGroupingRule" />
        </RuleState>
        <RuleState>
          <option name="name" value="StatusDashboardGroupingRule" />
        </RuleState>
      </list>
    </option>
    <option name="contentProportion" value="0.28370553" />
  </component>
```

然后在 `Component` 中添加下面的内容：

```xml
<option name="configurationTypes">  
    <set>  
        <option value="SpringBootApplicationConfigurationType" />  
    </set>  
</option> 
```

