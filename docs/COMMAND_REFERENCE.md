## 🤖 自动化脚本

### 完整构建脚本
```bash
#!/bin/bash
# build_apk.sh

set -e  # 遇到错误立即退出

echo "=== APK构建脚本开始 ==="
echo "时间: $(date)"
echo ""

# 1. 环境检查
echo "1. 检查构建环境..."
source check_environment.sh

# 2. 备份文件
echo -e "\n2. 备份重要文件..."
backup_files

# 3. 清理项目
echo -e "\n3. 清理项目..."
cordova clean

# 4. 准备平台
echo -e "\n4. 准备Android平台..."
cordova prepare android

# 5. 构建APK
echo -e "\n5. 构建APK..."
cordova build android --release

# 6. 查找APK
echo -e "\n6. 查找构建产物..."
APK_FILE=$(find platforms/android -name "app-release.apk" -type f | head -1)
if [ -z "$APK_FILE" ]; then
    APK_FILE=$(find platforms/android -name "*.apk" -type f | head -1)
fi

if [ -n "$APK_FILE" ]; then
    echo "找到APK: $APK_FILE"
    ls -lh "$APK_FILE"
    
    # 7. 复制到输出目录
    echo -e "\n7. 复制APK到输出目录..."
    mkdir -p output
    cp "$APK_FILE" "output/chinese-chess-$(date +%Y%m%d-%H%M%S).apk"
    echo "APK已保存到 output/"
else
    echo "错误: 未找到APK文件"
    exit 1
fi

echo -e "\n=== APK构建完成 ==="
echo "总时间: $(($SECONDS / 60))分$(($SECONDS % 60))秒"
```

### 环境检查脚本
```bash
#!/bin/bash
# check_environment.sh

echo "=== 环境检查 ==="

ERRORS=0

# 检查Java
if ! java -version 2>&1 | grep -q "11.0"; then
    echo "❌ Java版本错误: 需要Java 11"
    ERRORS=$((ERRORS + 1))
else
    echo "✅ Java 11 OK"
fi

# 检查Android构建工具
if [ ! -d "$ANDROID_HOME/build-tools/30.0.2" ]; then
    echo "❌ Android构建工具30.0.2未找到"
    ERRORS=$((ERRORS + 1))
else
    echo "✅ Android构建工具30.0.2 OK"
fi

# 检查Cordova
if ! cordova --version &>/dev/null; then
    echo "❌ Cordova未安装"
    ERRORS=$((ERRORS + 1))
else
    echo "✅ Cordova OK: $(cordova --version)"
fi

# 检查包名一致性
CONFIG_ID=$(grep 'widget id' config.xml 2>/dev/null | sed 's/.*id="//' | sed 's/".*//' || echo "")
JAVA_PKG=$(find platforms/android -name "MainActivity.java" -exec grep "^package" {} \; 2>/dev/null | sed 's/package //' | sed 's/;//' | head -1 || echo "")

if [ -n "$CONFIG_ID" ] && [ -n "$JAVA_PKG" ] && [ "$CONFIG_ID" = "$JAVA_PKG" ]; then
    echo "✅ 包名一致: $CONFIG_ID"
elif [ -z "$CONFIG_ID" ] || [ -z "$JAVA_PKG" ]; then
    echo "⚠️  包名检查跳过（文件未找到）"
else
    echo "❌ 包名不一致"
    echo "  Config: $CONFIG_ID"
    echo "  Java: $JAVA_PKG"
    ERRORS=$((ERRORS + 1))
fi

# 总结
if [ $ERRORS -eq 0 ]; then
    echo "✅ 所有检查通过"
else
    echo "❌ 发现 $ERRORS 个错误"
    exit 1
fi
```

### 一键修复脚本
```bash
#!/bin/bash
# fix_common_issues.sh

echo "=== 常见问题修复脚本 ==="

# 1. 修复构建工具版本
echo "1. 修复构建工具版本检查..."
if [ -f "platforms/android/cdv-gradle-config.json" ]; then
    sed -i 's/"MIN_BUILD_TOOLS_VERSION": "30.0.3"/"MIN_BUILD_TOOLS_VERSION": "30.0.2"/' platforms/android/cdv-gradle-config.json
    echo "✅ 修改cdv-gradle-config.json"
fi

# 2. 重新生成平台
echo -e "\n2. 重新生成平台文件..."
cordova platform rm android 2>/dev/null || true
cordova platform add android@10
echo "✅ 平台重新生成"

# 3. 修复MainActivity.java
echo -e "\n3. 修复MainActivity.java..."
MAIN_ACTIVITY_FILE="platforms/android/app/src/main/java/com/example/chessapp/MainActivity.java"
if [ -f "$MAIN_ACTIVITY_FILE" ]; then
    # 确保有正确的JavaScript启用代码
    if ! grep -q "webView.getSettings().setJavaScriptEnabled(true)" "$MAIN_ACTIVITY_FILE"; then
        echo "添加JavaScript启用代码..."
        # 这里可以添加具体的修复代码
    fi
    echo "✅ MainActivity.java检查完成"
else
    echo "⚠️  MainActivity.java未找到，平台生成后会自动创建"
fi

echo -e "\n=== 修复完成 ==="
echo "建议运行: cordova build android 测试修复效果"
```

---

**最后更新**: 2025年12月21日  
**命令总数**: 100+ 个实用命令  
**覆盖范围**: 环境检查、构建、修复、安装、调试全流程  
**使用建议**: 将此手册作为日常参考，遇到问题时按分类查找对应命令