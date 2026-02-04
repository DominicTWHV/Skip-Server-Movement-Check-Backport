# Fabric 1.20.1 Backport Configuration

This document outlines all the changes required to backport this mod from Fabric 1.21.7 to Fabric 1.20.1.

## Changes Made

### 1. gradle.properties
Updated to use Fabric 1.20.1 compatible versions:
- `minecraft_version`: 1.21.7 → 1.20.1
- `yarn_mappings`: 1.21.7+build.1 → 1.20.1+build.10
- `loader_version`: 0.16.14 → 0.14.21
- `fabric_version`: 0.128.1+1.21.7 → 0.92.2+1.20.1

### 2. build.gradle
- Changed Java version from 21 to 17 (required for Minecraft 1.20.1)
- Updated `fabric-loom` plugin configuration to use buildscript approach
- Plugin version: 1.10-SNAPSHOT → 1.4.4

### 3. src/main/resources/fabric.mod.json
Updated dependency versions:
- `fabricloader`: >=0.16.14 → >=0.14.21
- `minecraft`: >=1.21.7 → ~1.20.1
- `java`: >=21 → >=17

### 4. src/main/resources/skipservercheck.mixins.json
- Changed `compatibilityLevel` from JAVA_21 to JAVA_17

### 5. GitHub Actions CI/CD
Created `.github/workflows/build.yml` to automate JAR builds:
- Triggers on pushes to main/master branches
- Builds the mod using Gradle
- Uploads JAR artifacts
- Automatically attaches JARs to GitHub releases when tags are pushed

## Building the Mod

### Prerequisites
- Java 17 or higher
- Gradle 7.6 or higher (included via Gradle Wrapper)
- Internet access to maven.fabricmc.net

### Build Commands
```bash
# Make gradlew executable (Linux/Mac)
chmod +x gradlew

# Build the mod
./gradlew build

# Clean and rebuild
./gradlew clean build
```

The built JAR file will be located in `build/libs/` directory.

## Network Requirements

⚠️ **IMPORTANT**: This project requires access to the following repositories:
- `https://maven.fabricmc.net/` - Fabric Maven repository (required for fabric-loom plugin)
- `https://maven.maven.org/` - Maven Central
- `https://plugins.gradle.org/` - Gradle Plugin Portal

If you're in an environment with network restrictions, you may need to:
1. Configure a proxy
2. Use a mirror repository (e.g., BMCLAPI for China)
3. Request network access to these domains

## CI/CD Workflow

The GitHub Actions workflow will:
1. Run on every push to main/master branches
2. Run on pull requests targeting main/master
3. Can be manually triggered via workflow_dispatch
4. Build the mod JAR using Gradle
5. Upload artifacts for 30 days
6. Automatically attach JARs to GitHub releases when you create a tag

### Creating a Release
To create a release with automatic JAR attachment:
```bash
git tag v1.0.7-1.20.1
git push origin v1.0.7-1.20.1
```

## Compatibility

This mod is now configured for:
- Minecraft: 1.20.1
- Fabric Loader: 0.14.21 or higher
- Fabric API: 0.92.2+1.20.1
- Java: 17 or higher

## Testing

After building, test the mod by:
1. Installing it in a Minecraft 1.20.1 instance with Fabric Loader 0.14.21+
2. Verifying the mod loads without errors
3. Testing that server movement checks are properly disabled
