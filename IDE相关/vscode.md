### vscode调试

* vue项目

launch.json

```json
{
  // 使用 IntelliSense 了解相关属性。 
  // 悬停以查看现有属性的描述。
  // 欲了解更多信息，请访问: https://go.microsoft.com/fwlink/?linkid=830387
  "version": "0.2.0",
  "configurations": [
    {
      "type": "pwa-chrome",
      "request": "launch",
      "name": "Launch Chrome against localhost",
      "url": "http://localhost:8080",
      "webRoot": "${workspaceFolder}",
      "runtimeExecutable": "C:\\Program Files\\Google\\Chrome\\Application\\chrome.exe"
    }
  ]
}
```

默认创建的launch.json没有"runtimeExecutable"，无法运行调试的话加上即可

然后在vscode的调试界面运行即可，运行选项选对应的“name”

* java项目（坑，未解决）

launch.json

```json
```

