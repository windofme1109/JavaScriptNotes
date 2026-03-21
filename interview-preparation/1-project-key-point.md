# 1. 项目经历

1. 2022 年 5 月 - 2023 年 2 月 公共安全事务 Panda 查询平台

2. 2022 年 7 月 - 2023 年 5 月 字节跳动安全响应中心

3. 2023 年 2 月 - 2023 年 7 月 游戏安全运营平台

4. 2023 年 7 月 - 至今 云负载防护（主机防护）平台（CWPP）

5. 2023 年 11 月 - 至今 火山引擎云安全中心

6. 2025 年 5 月 - 至今 大模型安全评测平台

7. 其他项目
   - 安全培训中心（内部）
   - 高校 AI 安全挑战赛

# 2. 主要职责

1. 作为前端项目负责人，负责技术选型、需求拆解、预估工时与排期、需求开发等。

# 3. 项目重点与难点

## 1. 火山引擎云安全中心

### 1. 高级筛选表格

1. 响应式布局 ResizeObserver / useLayoutEffect

2. 基于事件订阅/发布机制的数据传动体系

3. useRef 避免重复创建实例

4. 多种表单项目

5. 级联搜索与高级搜索结合（数据同步）

6. 内外数据同步优化

#### 1. 数据更新流程：

1. 在代理类使用 state 存储表单状态。使用 event 作为事件中心，提供事件订阅、发布、移除等能力。

2. 使用 Controller 组件包裹表单项组件，Controller 组件会向表单项组件注入 value 和 change 事件处理函数，使得表单项组件成为受控组件。每个 Controller 会绑定一个 key，这个 key 作为 Controller 的唯一标识，也是作为订阅事件的事件名称。在 Controller 组件内部订阅更新事件，因为只有一个事件中心，Controller 订阅的事件都会在事件中心中注册。当数据更新时，向对应的 Controller 发送通知， Controller 收到通知后，立即更新组件。

4. 当某个表单项数据更新后，通过 Controller 的 change 处理函数同步更新至代理类的 state 中。待内部的 state 更新完成以后，在借由外部组件的  change 处理函数将最新的 state 同步至外部组件中。

5. 外部组件的数据更新后，首先同步更新到代理类的 state 中，在更新过程中，会对新旧数据进行比较，找出需要更新的字段与值。更新完成后，事件中心会根据更新的字段列表，逐一通知对应的 Controller 完成更新。

6. 代理类中的 state 更新时，同步更新 url 查询字符串。

### 2. 跨服务授权 hook

1. 内部封装一个状态，用来控制授权提示弹窗的开启与关闭，不需要在外边定义状态。

2. 内部定义一个轮询函数，用来获取授权状态，该函数为 Promise 函数，可以在 async 函数内部调用。
```typescript
    const pollingAuthFetcher = () => {
        const innerPollingAuthFetcher = (resolve: (value?: ServiceAuthInfo) => void) => {
            const timer = setTimeout(() => {
                authFetcher().then((res) => {
                    const { HasAuthorize } = res;
                    // 清除定时器
                    clearTimeout(timer);

                    if (!HasAuthorize) {
                        innerPollingAuthFetcher(resolve);
                    } else {
                        resolve(res);
                    }
                });
            }, pollingInterval);
        };

        return new Promise<ServiceAuthInfo>((resolve) => {
            innerPollingAuthFetcher(resolve);
        });
    };
```

3. useMemo 缓存数据。

### 3. 复杂表单项目联动

1. memo 包裹组件提升性能。

2. useState 保存内部状态。

3. useEffect 回填数据与更新表单项状态。



## 2. CWPP


## 3. 大模型安全评测平台

### 1. 基于 React-Router 自定路由守卫实现了路由级别的权限控制

1. 使用 atom 定义全局状态，保存权限信息。

2. 定义路由路径与权限信息。

3. 自定义权限 hook 函数用来检查某个路由是否具有相关权限：
   - 不可见
   - 加锁（涉及到新购）

4. 自定义路由守卫组件，在注册路由组件时，使用路由守卫组件包裹真正的路由组件。

5. 路由守卫组件内部，会根据当前被包裹的组件判断是否有相关的权限。

6. 菜单导航栏使用权限 hook 函数决定某个菜单是否加锁或者是隐藏。

7. 路由守卫组件：在渲染真正的路由组件前，对当前的路由进行一些操作，如权限判断、登录判断等。React-Router 并没有提供路由守卫相关的 hook，所以需要自行处理。目前的处理方式是：在注册路由组件的时候，使用路由守卫组件包裹真正的组件。 