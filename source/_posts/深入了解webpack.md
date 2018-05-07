---
title: 深入了解webpack
date: 2018/05/01 22:04
tags: [前端,工具]
categories: 前端
comments: true
---
webpack是目前最为流行的打包工具之一，其配置简单，功能强大，拥有丰富的加载器和插件系统，为前端开发者提供了诸多便利。笔者默认各位看官在阅读本章之前已经有了一定的使用经验，所以对webpack的使用方式不做赘述。

我们知道webpack有如下四个基本配置：
- 入口
- 出口
- 加载器
- 插件

我们将在解读源码过程中了解这四个基本配置，及其背后的工作原理。从`github`上将[webpack源码](https://github.com/webpack/webpack/)克隆至本地。我们首先看看`lib/webpack.js`这个文件，此文件为`webpack`的入口文件，我们从中可以了解`webpack`执行的流程。
```js
const webpack = (options, callback) => {
	const webpackOptionsValidationErrors = validateSchema(
		webpackOptionsSchema,
		options
	);
	if (webpackOptionsValidationErrors.length) {
		throw new WebpackOptionsValidationError(webpackOptionsValidationErrors);
	}
	let compiler;
	if (Array.isArray(options)) {
		compiler = new MultiCompiler(options.map(options => webpack(options)));
	} else if (typeof options === "object") {
		options = new WebpackOptionsDefaulter().process(options);

		compiler = new Compiler(options.context);
		compiler.options = options;
		new NodeEnvironmentPlugin().apply(compiler);
		if (options.plugins && Array.isArray(options.plugins)) {
			for (const plugin of options.plugins) {
				plugin.apply(compiler);
			}
		}
		compiler.hooks.environment.call();
		compiler.hooks.afterEnvironment.call();
		compiler.options = new WebpackOptionsApply().process(options, compiler);
	} else {
		throw new Error("Invalid argument: options");
	}
	if (callback) {
		if (typeof callback !== "function")
			throw new Error("Invalid argument: callback");
		if (
			options.watch === true ||
			(Array.isArray(options) && options.some(o => o.watch))
		) {
			const watchOptions = Array.isArray(options)
				? options.map(o => o.watchOptions || {})
				: options.watchOptions || {};
			return compiler.watch(watchOptions, callback);
		}
		compiler.run(callback);
	}
	return compiler;
};
```
从中我们能够了解到一个简单的流程：
- 参数验证
- 创建 `Compiler` (编译器)对象
- 注册并执行 `NodeEnvironmentPlugin`
- 执行钩子 `environment` 里的方法
- 执行钩子 `afterEnvironment` 里的方法
- 注册并执行各种插件
- 将 `compiler` 向外导出

这个流程比较简单清晰，我们需要对 `Compiler` 的实现做深究。大致了解下 `Compiler` 的实现：
```js
class Compiler extends Tapable {
	constructor(context) {
        super();
		this.hooks = {
            // ...
        };
        this._pluginCompat.tap("Compiler", options => {
            // ...
        });
        // ... 
        this.resolvers = {
            normal: {
                // ...
            },
            loader: {
                // ...
            },
            context: {
                // ...
            }
        };
        // ...
    }
    watch(watchOptions, handler) {
        // ...
    }
    run(callback) {
        // ...
    }
    runAsChild(callback) {
        // ...
    }
    purgeInputFileSystem() {
        // ...
    }
    emitAssets(compilation, callback) {
        // ...
    }
    emitRecords(callback) {
        // ...
    }
    readRecords(callback) {
        // ...
    }
    createChildCompiler(
		compilation,
		compilerName,
		compilerIndex,
		outputOptions,
		plugins
	) {
        // ...
    }
    isChild() {
        // ...
    }
    createCompilation() {
        // ...
    }
    newCompilation(params) {
        // ...
    }
    createNormalModuleFactory() {
        // ...
    }
    createContextModuleFactory() {
        // ...
    }
    newCompilationParams() {
        // ...
    }
    compile(callback) {
        // ...
    }
}
```