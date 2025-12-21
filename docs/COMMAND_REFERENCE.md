# 命令参考手册

## 📋 命令分类索引

### 快速导航
- [环境检查命令](#环境检查命令)
- [构建相关命令](#构建相关命令)
- [问题修复命令](#问题修复命令)
- [文件操作命令](#文件操作命令)
- [安装与部署命令](#安装与部署命令)
- [调试与诊断命令](#调试与诊断命令)
- [自动化脚本](#自动化脚本)

## 🔍 环境检查命令

### 基础环境检查
```bash
# 检查Java版本（必须为11）
java -version

# 检查Node.js版本
node --version
npm --version

# 检查Cordova版本
cordova --version
which cordova

# 检查系统架构
uname -a
uname -m
```

### Android SDK检查
```bash
# 检查Android SDK路径
echo $ANDROID_HOME
echo $ANDROID_SDK_ROOT

# 检查构建工具版本
ls -la $ANDROID_HOME/build-tools/

# 检查平台版本
ls -la $ANDROID_HOME/platforms/

# 检查环境变量
env | grep -i "android\|java\|gradle"
```

### Cordova环境检查
```bash
# 检查Cordova要求
cordova requirements

# 列出已安装平台
cordova platform list

# 检查插件列表
cordova plugin list

# 检查项目配置
cordova info
```

### 完整环境检查脚本
```bash
#!/bin/bash
# complete_env_check.sh

echo "=== 完整环境检查 ==="
echo "时间: $(date)"
echo ""

echo "1. 系统信息:"
uname -a
echo ""

echo "2. Java环境:"
java -version 2>&1
echo "JAVA_HOME: $JAVA_HOME"
echo ""

echo "3. Node.js环境:"
echo "Node: $(node --version)"
echo "npm: $(npm --version)"
echo ""

echo "4. Cordova环境:"
echo "Cordova: $(cordova --version)"
echo "路径: $(which cordova)"
echo ""

echo "5. Android SDK:"
echo "ANDROID_HOME: $ANDROID_HOME"
echo "构建工具:"
ls $ANDROID_HOME/build-tools/ 2>/dev/null || echo "未找到构建工具"
echo "平台:"
ls $ANDROID_HOME/platforms/ 2>/dev/null || echo "未找到平台"
echo ""

echo "6. Gradle环境:"
gradle --version 2>&1 | head -5
echo ""

echo "7. 存储空间:"
df -h .
echo ""

echo "=== 检查完成 ==="
```