1. 使用React.lazy()加载组件，是报错 'Uncaught Invariant Violation: Unable to find node on an unmounted compone'

- 原因：React.lazy 需要和 suspense 一起使用
```
const Foo = React.lazy(() => import('../components/Foo'));

export default class LazyPage extends Component {
    render() {
        return (
            <div>
                <Suspense fallback={<div>loading...</div>}>
                    <Foo/>
                </Suspense>
            </div>
        )
    }
};
```