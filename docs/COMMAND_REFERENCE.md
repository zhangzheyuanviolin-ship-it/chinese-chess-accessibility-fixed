## 🔧 问题修复命令

### 包名不一致修复
```bash
# 检查当前包名配置
grep 'widget id' config.xml | sed 's/.*id="//' | sed 's/".*//'

# 修改config.xml包名
sed -i 's/<widget id="旧包名"/<widget id="com.example.chessapp"/' config.xml

# 重新生成平台文件
cordova platform rm android
cordova platform add android@10

# 验证包名一致性
echo "Config: $(grep 'widget id' config.xml | sed 's/.*id="//' | sed 's/".*//')"
echo "Java: $(find platforms/android -name 'MainActivity.java' -exec grep '^package' {} \; | sed 's/package //' | sed 's/;//')"
```

### 构建工具版本修复
```bash
# 修改cdv-gradle-config.json
sed -i 's/"MIN_BUILD_TOOLS_VERSION": "30.0.3"/"MIN_BUILD_TOOLS_VERSION": "30.0.2"/' platforms/android/cdv-gradle-config.json

# 备份cordova.gradle
cp platforms/android/cordova.gradle platforms/android/cordova.gradle.backup

# 修改cordova.gradle第一处（版本检查）
# 编辑文件，在第185-189行附近添加特殊处理
# if (minBuildToolsVersionString == "30.0.3" && highestBuildToolsVersion.getOriginalString() == "30.0.2") {
#     println "WARNING: Using build tools 30.0.2 instead of required 30.0.3"
#     return highestBuildToolsVersion
# }

# 修改cordova.gradle第二处（函数内检查）
# 在doFindLatestInstalledBuildTools函数中添加相同处理
```

### Java文件问题修复
```bash
# 删除问题Java文件
rm -f platforms/android/app/src/main/java/com/example/chessapp/MainActivity.java

# 重新生成平台
cordova platform rm android
cordova platform add android@10

# 验证Java文件生成
find platforms/android -name "MainActivity.java" -type f
```

### Cordova版本修复
```bash
# 移除错误版本
cordova platform rm android

# 添加兼容版本（必须）
cordova platform add android@10

# 验证版本
cordova platform list | grep android
```