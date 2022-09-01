---
layout: post
title:   变更gems源到ruby-china相关命令
date:   2022-08-31 19:24:02
---

查看一个源

gem source -l


删除一个源

gem source -r https://rubygems.org/

显示：https://rubygems.org/ removed from sources， 注意，最后这个斜杠不能少



添加源的命令

gem source --add https://gems.ruby-china.com/

显示：https://gems.ruby-china.com/ added to sources


更新一个源

gem source -u

显示：source cache successfully updated

