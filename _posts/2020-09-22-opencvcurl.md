---
layout: post
title: curl (18) transfer closed with outstanding read data remaining
comments: true
tags: [problem]
---

编译NVIDIA_DALI时遇到"curl (18) transfer closed with outstanding read data remaining"

原因是curl下载opencv源码时,github被墙导致下载失败

需要将curl的目标地址改为
"https://www.bzblog.online/opencv/$VERSION"即可
