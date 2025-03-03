# 2025-03-03

## The Maven Publish Plugin

https://docs.gradle.org/current/userguide/publishing_maven.html

## PublishingExtension

https://docs.gradle.org/current/dsl/org.gradle.api.publish.PublishingExtension.html

## MavenPublication

https://docs.gradle.org/current/dsl/org.gradle.api.publish.maven.MavenPublication.html

## MavenArtifactRepository

https://docs.gradle.org/current/dsl/org.gradle.api.artifacts.repositories.MavenArtifactRepository.html

## PublishToMavenLocal

https://docs.gradle.org/current/javadoc/org/gradle/api/publish/maven/tasks/PublishToMavenLocal.html

## complete publishing example

https://docs.gradle.org/current/userguide/publishing_maven.html#publishing_maven:complete_example

# Publish your library | Android Studio

https://developer.android.com/build/publish-library?hl=zh-cn

# 总结

## 1. 在`build.gradle`中加入插件

```
plugins {
    alias(libs.plugins.android.library)
    //必须要有kotlin插件
    alias(libs.plugins.kotlin.android)
    //核心点
    id 'maven-publish'
}
```

或者 

```
apply plugin: 'com.android.library'
//必须要有kotlin插件
apply plugin: 'kotlin-android'
//核心点
apply plugin: 'maven-publish'
```

## 2. 在`build.gradle`中加入`publishing`配置

```
afterEvaluate { //必须要放在 afterEvaluate 内
    publishing {
        publications {
            maven(MavenPublication) {
                //配置基础信息
                groupId = 'com.angcyo'
                artifactId = 'mylibrary'
                version = '1.0'

                //关键点
                from components.release
            }
        }
        repositories {
            //mavenLocal()
            //配置输出目录
            maven {
                // change to point to your repo, e.g. http://my.org/repo
                url = layout.buildDirectory.dir('repo')
            }
        }
    }
}
```

## 3. 运行构建任务即可生成对应的pom信息

`./gradlew publish` 或 `./gradlew publishToMavenLocal`