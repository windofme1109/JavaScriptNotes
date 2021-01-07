# 在 TypeScript 中引入非 ts 类型的文件

1. 正常来说，我们在导入一个文件时，TS 只认识 `js`、`jsx`、`ts`、`tsx` 文件，对于其他类型的文件，TS 都不认识，这样导入就会报错：Module not found。

2. 在这种情况下，我们要自己书写类型定义文件：`.d.ts`。在类型定义文件中，声明我们要引入的模块（文件）类型。

3. 以图片文件为例，如果我们直接引入：`import img from './public/gold.jpg'`，会提示错误。然后，我们新建一个类型声明文件：`images.d.ts`，内容如下：
   ```
      declare module "*.jpg" {
          const jpg: any;
          export = jpg;
      };
   ```
4. 我们为 jpg 文件声明一个新模块，该模块指定以 `.jpg` 结尾的任何导入并将模块的内容定义为 `any`。

5. 这样我们可以顺利的导入 jpg 文件了。同理，还可以添加其他类型的图片文件：
   ```
     declare module "*.png";

     declare module "*.jpeg";

     declare module "*.gif";

     declare module "*.svg";   
   ```

6. 对于 `css`、`json`、`scss` 等文件都可以通过定义类型声明文件来解决不能被 TS 识别的问题。

7. 如果我们在 tsconfig.json 配置了 `include` 字段，那么类型声明文件只能放置在 `include` 属性所配置的文件夹下。如果没有配置，将其放在项目根路径（与 tsconfig.json 同级）或者 src 目录下均可。