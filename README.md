# react-router-dom-v6


### 路由配置

      <Router>
            <Routes>
                <Route path='/foo' element={Foo} />
                <Route path='/bar' element={Bar} />
            </Routes>
        </Router>


### 路由跳转
    在跳转路由时，如果路径是/开头的则是绝对路由，否则为相对路由，即相对于“当前URL”进行改变
        1.Link组件
            <Link to='foo'>to foo</Link>
        2.NavLink组件
            <NavLink
                style={ (isActive) => ({color: isActive ? 'red' : '#fff'}) }
            >Click here</NavLink>
        3.函数式编程导航
        const navigate = useNavigate();
        <div onClick={() => navigate('/foo')}>跳转</div>

### 路由参数
        1.路径参数
        在Route组件中的path属性中定义路径参数
        在组件内通过useParams hook访问路径参数

        <Route path='/foo/:id' element={Foo} />
        const params = useParams(); {id:}

        在以前版本中，组件的props会包含一个match对象，在其中可以取到路径参数。但在最新的6.x版本中，无法从props获取参数。
        并且，针对类组件的withRouter高阶组件已被移除。因此对于类组件来说，使用参数有两种兼容方法：
        将类组件改写为函数组件
        字节写一个HOC来包裹类组件，用useParams获取参数后通过props传入原本的类组件
        
        2.search参数
        查询参数不需要在路由中定义
        使用useSearchParams hook来访问查询参数。其用法和useState类似，会返回当前对象和更改它的方法
        更改searchParams时，必须传入所有的查询参数，否则会覆盖已有参数
        const [searchParams, setSearchParams] = useSearchParams();
        searchParams.get('id')

### 路由规则
    1.嵌套路由
        通过嵌套的书写Route组件实现对嵌套路由的定义
        <Routes>
            <Route path="/" element={<Home/>} ></Route>
            <Route path="/father" element={<Father/>} >
            <Route path='child' element={<Child/>}></Route>
            </Route>
        </Routes>

        在父组件中使用Outlet来显示匹配到的子组件
        import {Outlet} from 'react-router-dom';
        function Father(){
            return (
                <div>
                    // ... 自己组件的内容
                    // 留给子组件Child的出口
                    <Outlet />
                </div>
            )
        }
    
    2. 默认路由
    定义：在嵌套路由中，如果URL仅匹配了父级URL，则Outlet中会显示带有index属性的路由
    <Routes>
        <Route path='/foo' element={Foo}>
            <Route index element={Default}></Route>
            <Route path='bar' element={Bar}></Route>
        </Route>
    </Routes>
    当url为/foo时：Foo中的Outlet会显示Default组件
    当url为/foo/bar时：Foo中的Outlet会显示为Bar组件

    3.全匹配路由
    定义： path属性取值为*时，可以匹配任何（非空）路径，同时该匹配拥有最低的优先级。可以用于设置404页面。
    <Routes>
    <Route path='/foo' element={Foo}>
        <Route path='bar' element={Bar}></Route>
        <Route path='*' element={NotFound}></Route>
    </Route>


    4.多组路由
    通常，一个应用中只有一个Routes组件。但根据实际需要也可以定义多个路由出口（如侧边栏和主页面都要随URL而变化）
    <Router>
        <SideBar>
            <Routes>
                <Route></Route>
            </Routes>
        </SideBar>
        <Main>
            <Routes>
                <Route></Route>
            </Routes>
        </Main>
    </Router>

    5.重定向
    当在某个路径/a下，要重定向到路径/b时，可以通过Navigate组件进行重定向到其他路径 等价于以前版本中的Redirect组件
    import { Navigate } from 'react-router-dom';
    function A(){
        return (
            <Navigate to="/b" />
        )
    }

</Routes>

###