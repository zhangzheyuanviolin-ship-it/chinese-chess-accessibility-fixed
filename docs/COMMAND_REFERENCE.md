## 🐛 调试与诊断命令

### 构建日志分析
```bash
# 保存详细构建日志
cordova build android --verbose 2>&1 | tee build.log

# 提取错误信息
grep -i "error\|fail\|exception" build.log

# 查看错误上下文
grep -B5 -A5 "error:" build.log

# 统计错误类型
grep -o "error:.*" build.log | sort | uniq -c

# 查看构建时间
grep "BUILD SUCCESSFUL\|BUILD FAILED" build.log
```

### 运行时调试
```bash
# 查看应用日志
adb logcat | grep com.example.chessapp

# 查看WebView日志
adb logcat | grep -i "webview\|chromium"

# 查看JavaScript错误
adb logcat | grep -i "console\|javascript"

# 清除日志并重新开始
adb logcat -c && adb logcat | grep com.example.chessapp
```

### 性能分析
```bash
# 查看构建时间
time cordova build android

# 查看内存使用
adb shell dumpsys meminfo com.example.chessapp

# 查看CPU使用
adb shell top -n 1 | grep com.example.chessapp

# 查看应用启动时间
adb shell am start -W -n com.example.chessapp/.MainActivity
```

### 网络调试
```bash
# 查看网络连接
adb shell netstat -tulpn | grep com.example.chessapp

# 查看WebView网络请求
adb logcat | grep -i "http\|https\|network"

# 启用WebView调试
adb shell setprop debug.webview.remote_debugging 1
```