## 🛠️ 构建相关命令

### 标准构建流程
```bash
# 1. 清理项目
cordova clean

# 2. 准备平台文件
cordova prepare android

# 3. 构建调试版APK
cordova build android
# 或
cordova build android --debug

# 4. 构建发布版APK
cordova build android --release
```

### Gradle直接构建
```bash
# 进入Android平台目录
cd platforms/android

# 清理构建
./gradlew clean

# 构建调试版
./gradlew assembleDebug

# 构建发布版
./gradlew assembleRelease

# 查看构建任务
./gradlew tasks
```

### 构建选项
```bash
# 启用详细输出
cordova build android --verbose

# 指定构建架构
cordova build android -- --gradleArg=-PcdvBuildArch=arm64

# 禁用压缩（调试用）
cordova build android -- --gradleArg=-PcdvEnableCompression=false

# 指定构建工具版本
cordova build android -- --gradleArg=-Pandroid.buildToolsVersion=30.0.2
```

### 构建产物查找
```bash
# 查找所有APK文件
find platforms/android -name "*.apk" -type f

# 查找调试版APK
find platforms/android -name "app-debug.apk" -type f

# 查找发布版APK
find platforms/android -name "app-release.apk" -type f

# 查看APK信息
ls -lh platforms/android/app/build/outputs/apk/*/*.apk
```