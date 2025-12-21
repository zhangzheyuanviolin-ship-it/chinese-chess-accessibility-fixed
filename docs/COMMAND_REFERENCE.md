## 📱 安装与部署命令

### APK文件传输
```bash
# 复制APK到Android存储
cp platforms/android/app/build/outputs/apk/debug/app-debug.apk /sdcard/

# 通过ADB传输
adb push platforms/android/app/build/outputs/apk/debug/app-debug.apk /sdcard/

# 验证文件传输
adb shell ls -lh /sdcard/app-debug.apk

# 计算文件哈希（验证完整性）
md5sum platforms/android/app/build/outputs/apk/debug/app-debug.apk
adb shell md5sum /sdcard/app-debug.apk
```

### APK安装命令
```bash
# 基本安装
adb install /sdcard/app-debug.apk

# 替换安装（保留数据）
adb install -r /sdcard/app-debug.apk

# 授予所有权限安装
adb install -r -g /sdcard/app-debug.apk

# 安装到特定用户
adb install --user 0 /sdcard/app-debug.apk
```

### 应用管理命令
```bash
# 列出已安装应用
adb shell pm list packages | grep chess

# 卸载应用
adb uninstall com.example.chessapp

# 强制卸载
adb uninstall -k com.example.chessapp

# 清除应用数据
adb shell pm clear com.example.chessapp

# 启动应用
adb shell am start -n com.example.chessapp/.MainActivity

# 停止应用
adb shell am force-stop com.example.chessapp
```

### 安装验证
```bash
# 检查应用是否安装
adb shell pm list packages | grep -q "com.example.chessapp" && echo "已安装" || echo "未安装"

# 检查应用版本
adb shell dumpsys package com.example.chessapp | grep versionName

# 检查应用权限
adb shell dumpsys package com.example.chessapp | grep permission

# 检查应用运行状态
adb shell ps | grep com.example.chessapp
```