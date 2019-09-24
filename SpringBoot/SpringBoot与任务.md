## 异步任务
### demo
在启动类上添加@EnableAsync:开启异步注解
controller
```java
@RestController
public class AsyncController {

    @Autowired
    AsyncService asyncService;

    @GetMapping("/hello")
    public String hello() {
        asyncService.hello();
        return "success";
    }
}
```
service
```java
@Service
public class AsyncService {

    @Async //告诉spring这是一个异步方法
    public void hello() {
        try {
            Thread.sleep(1000);
        } catch (Exception e) {
            e.printStackTrace();
        }
        System.out.println("处理数据中...");
    }
}
```
### 定时任务
| 字段 | 允许值             | 允许的特殊字符 |
| ---- | --------------------- | -------------- |
| 秒  | 0-59                  | ,-/*           |
| 分  | 0-59                  | ,-/*           |
| 小时 | 0-23                  | ,-/*           |
| 日期 | 1-31                  | ,-*?/LWC       |
| 月份 | 1-12                  | ,-/*           |
| 星期 | 0-7或SUN-SAT,0,7是SUN | ,-*?/LC#       |

---
| 特殊字符 | 代表含义               |
| -------- | -------------------------- |
| ,        | 枚举                     |
| -        | 区间                     |
| /        | 步长                     |
| *        | 任意                     |
| ?        | 日/星期冲突匹配    |
| L        | 最后                     |
| W        | 工作日                  |
| C        | 和Calendar联系后计算过的值 |
| #        | 星期  4#2:第2个星期四|

### demo
在启动类上添加@EnableScheduling//开启基于注解的定时任务
```java
   /**
     * on the second as well as minute, hour, day of month, month and day of week.
     * such as {@code "0 * * * * MON-FRI"}:周一到周五每分钟0秒启动
     * [* * * * * MON-FRI] 周一到周五每秒执行一次
     * [0 0/5 14,18 * * ?] 每天14点整和18点整,每隔5分钟执行一次
     * [0 15 10 ? * 1-6] 每个月的周一至周六10:15分执行一次
     * [0 0 2 ? * 6L] 每个月的最后一个周六凌晨2点执行一次
     * [0 0 2 LW * ?] 每个月最后一个工作日凌晨2点执行一次
     * [0 0 2-4 ? * 1#1] 每个月的第一个周一凌晨2点到4点期间,每个整点执行一次
     */
    @Scheduled(cron = "0/4 * * * * MON-FRI")
    public void hello() {
        System.out.println("hello");
    }
```

资料参考:
[cron表达式示例](https://linuxtools-rst.readthedocs.io/zh_CN/latest/tool/crontab.html)