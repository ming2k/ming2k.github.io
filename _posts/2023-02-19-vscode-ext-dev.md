# Visual Studio Code Extension Dev

本教程致力于Visual Studio Code插件教程开发。

## Start

安装`Yeoman`并生成样板代码：

```sh
npm install -g yo generator-code
```

生成样板代码：

```sh
yo code
# ? What type of extension do you want to create? New Extension (TypeScript)
# ? What's the name of your extension? HelloWorld
### Press <Enter> to choose default for all options below ###

# ? What's the identifier of your extension? vscx-demo
# ? What's the description of your extension? Visual studio code extension demo.
# ? Enable stricter TypeScript checking in 'tsconfig.json'? Yes
# ? Setup linting using 'tslint'? Yes
# ? Initialize a git repository? Yes
# ? Which package manager to use? npm

code ./vscx-demo
```

按下`F5`或从`Run and Debug`中运行插件`run extension`，打开新的vscode窗口作为插件容器。

在新的VS Code中使用快捷键`cmd + shift + p`打开`Palette`运行命令`Hello World`会在右下角弹出

```
Hello World from vscx-demo!
```

`package.json`中标注了代码的入口文件：

```
{
	"main": "./out/extension.js",
}
```

我们可以看下`src/extension.js`：

```ts
// The module 'vscode' contains the VS Code extensibility API
// Import the module and reference it with the alias vscode in your code below
import * as vscode from 'vscode';

// This method is called when your extension is activated
// Your extension is activated the very first time the command is executed
export function activate(context: vscode.ExtensionContext) {

	// Use the console to output diagnostic information (console.log) and errors (console.error)
	// This line of code will only be executed once when your extension is activated
	console.log('Congratulations, your extension "vsc-pdf-viewer" is now active!');

	// The command has been defined in the package.json file
	// Now provide the implementation of the command with registerCommand
	// The commandId parameter must match the command field in package.json
	let disposable = vscode.commands.registerCommand('vsc-pdf-viewer.helloWorld', () => {
		// The code you place here will be executed every time your command is executed
		// Display a message box to the user
		vscode.window.showInformationMessage('Hello World from vsc-pdf-viewer!');
	});

	context.subscriptions.push(disposable);
}

// This method is called when your extension is deactivated
export function deactivate() {}
```

上述样例解释的十分全面，耐心看看。