翻译自retrofit官方网站：http://square.github.io/retrofit/

1、将网络HTTP请求转换为java的接口
public interface GitHubService {
  @GET("users/{user}/repos")
  Call<List<Repo>> listRepos(@Path("user") String user);
}

2、通过Retrofit类产生GitHubService的接口对象
Retrofit retrofit = new Retrofit.Builder()
    .baseUrl("https://api.github.com/")
    .build();
GitHubService service = retrofit.create(GitHubService.class);

3、每一个来之GitHubService接口里面的call接口都是有锁的，与远程服务器连接安全
如：Call<List<Repo>> repos = service.listRepos("octocat");

用注释来描述HTTP请求：
1）URL参数可替换同时可查询支持参数类型
2）将对象转化为请求体（带json或者协议缓冲区...）
3)多请求体和文件更新

API理解
在接口方法和参数上的注释暗示请求是怎么处理的

请求方法
每一个方法都有一个注释，提供请求方式和关联URL。有5种内置的请求方法：GET, POST, PUT, DELETE, 和 HEAD。关联资源的URL在注释里面指定。
@GET("users/list")
当然也可以指定查询参数
@GET("users/list?sort=desc")

URL操作
一个请求的URL可以在方法里面用替代代码块和参数动态更新。替代代码块一般是由{}包裹，关联的方法参数必须用相同字符串带“@path”标记配合使用
@GET("group/{id}/users")
Call<List<User>> groupList(@Path("id") int groupId);

可以添加查询参数
@GET("group/{id}/users")
Call<List<User>> groupList(@Path("id") int groupId, @Query("sort") String sort);

可以用map来呈放复杂的参数综合体
@GET("group/{id}/users")
Call<List<User>> groupList(@Path("id") int groupId, @QueryMap Map<String, String> options);

