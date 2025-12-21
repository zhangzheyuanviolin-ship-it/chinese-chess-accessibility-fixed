## 📁 文件操作命令

### 文件查找与验证
```bash
# 查找配置文件
find . -name "config.xml" -type f
find . -name "cdv-gradle-config.json" -type f
find . -name "cordova.gradle" -type f

# 查找Java文件
find platforms/android -name "*.java" -type f
find platforms/android -name "MainActivity.java" -type f

# 检查文件权限
ls -la platforms/android/app/src/main/java/com/example/chessapp/

# 检查文件内容
head -50 platforms/android/app/src/main/java/com/example/chessapp/MainActivity.java
```

### 文件备份与恢复
```bash
# 备份重要文件
backup_files() {
    mkdir -p backup/$(date +%Y%m%d)
    cp config.xml backup/$(date +%Y%m%d)/
    cp platforms/android/cdv-gradle-config.json backup/$(date +%Y%m%d)/
    cp platforms/android/cordova.gradle backup/$(date +%Y%m%d)/
    cp platforms/android/app/src/main/java/com/example/chessapp/MainActivity.java backup/$(date +%Y%m%d)/ 2>/dev/null || true
    echo "文件已备份到 backup/$(date +%Y%m%d)/"
}

# 恢复文件
restore_files() {
    if [ -d "backup/$1" ]; then
        cp backup/$1/config.xml .
        cp backup/$1/cdv-gradle-config.json platforms/android/
        cp backup/$1/cordova.gradle platforms/android/
        cp backup/$1/MainActivity.java platforms/android/app/src/main/java/com/example/chessapp/ 2>/dev/null || true
        echo "文件已从 backup/$1/ 恢复"
    else
        echo "备份目录 backup/$1/ 不存在"
    fi
}
```

### 文件差异比较
```bash
# 比较文件差异
diff platforms/android/cordova.gradle platforms/android/cordova.gradle.backup

# 查看文件修改内容
git diff platforms/android/cordova.gradle

# 检查文件编码
file platforms/android/app/src/main/java/com/example/chessapp/MainActivity.java

# 检查文件行数
wc -l platforms/android/app/src/main/java/com/example/chessapp/MainActivity.java
```