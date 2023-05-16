## tsconfig.json

```json
{
    "compileOnSave": true,
    "compilerOptions": {
        "module": "commonjs",
        "noImplicitAny": true,
        "removeComments": true,
        "preserveConstEnums": true,
        "sourceMap": true
    },
    "typeRoots" : ["./typings"],
    "types" : ["node", "lodash", "express"],
    "files": [
        "core.ts",
        "sys.ts",
        "commandLineParser.ts",
        "tsc.ts",
        "diagnosticInformationMap.generated.ts"
    ],
    "include": [
        "src/**/*"
    ],
    "exclude": [
        "node_modules",
        "**/*.spec.ts"
    ]
}
```

- `files`指定要编译的源代码列表，只能是相对或绝对文件路径
- 使用`include`和`exclude`，支持通配符：默认支持`.ts`、`.tsx`、`.d.ts`
  - **优先级**：
    1. `files`列出的文件会被编译
    2. `include`中匹配的文件，如果没有被`exclude`匹配，会被编译
    3. `exclude`匹配的文件不会被编译
  - **规则**：
    - `*`匹配0或多个字符
    - `？`匹配一个任意字符
    - `**/`递归匹配任意子目录
- `@types`、`typeRoots`、`types`：
  - 默认所有可见的`@types`包会被包含
  - 如果指定`typeRoots`，只有它指定的路径下面的包会被包含
  - 如果指定`types`,只有被列出的包才会被包含
- 使用`extends`继承配置
- 最顶层设置`compileOnSave`，在IDE中保存文件，会根据`tsconfig.json` 重新生成文件

## 编译选项

