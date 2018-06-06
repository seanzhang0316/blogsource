---
title: "IDEA安装路径"
date: 2016-06-04T20:45:38+08:00
draft: false
---

之前初次接触intellij idea，免不了各种折腾，清空配置、插件什么的。写份安装路径，方便自己也方便大家。

### Windows

#### 默认安装路径：

- **Windows Vista, 7, 8, 9**

  ```
  <SYSTEM DRIVE>\Users\<USER ACCOUNT NAME>\.<PRODUCT><VERSION>
  ```

- **Windows XP:**

  ```
  <SYSTEM DRIVE>\Documents and Settings\<USER ACCOUNT NAME>\.<PRODUCT><VERSION>
  ```

#### 不同版本的路径：

- **IntelliJ IDEA 14 Ultimate Edition:**

  ```
  c:\Users\USER ACCOUNT NAME\.IntelliJIdea14\
  ```

- **IntelliJ IDEA 14 Community Edition:**

  ```
  c:\Users\USER ACCOUNT NAME\.IdeaIC14\
  ```

### Linux 及 UN*X 系统

```
~/.<PRODUCT><VERSION>
```

～是你的home目录，例如/home/sean

### Mac OS X

- **Configuration:**

  ```
  ~/Library/Preferences/<PRODUCT><VERSION>
  ```

- **Caches:**

  ```
  ~/Library/Caches/<PRODUCT><VERSION>
  ```

- **Plugins:**

  ```
  ~/Library/Application Support/<PRODUCT><VERSION>
  ```

- **Logs:**

  ```
  ~/Library/Logs/<PRODUCT><VERSION>
  ```

**PRODUCT**: 产品名称，例如**IntelliJIdea** (IntelliJ IDEA Ultimate Edition)