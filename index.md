需求场景
比如，现在工作流系统activiti和spring服务是相互独立，activiti服务不受springIOC管理，那么怎么在activiti里面应用spring提供的服务，这个就需要借助下面的这个小工具，见到这个工具类，总结下来，方便大家后期使用，我测试了在工作流系统调用spring管理的mapper依赖。其他场景如果没有用，那就当给大家提供一种思路吧！
代码
工具类代码：
[java] view plain copy
<span style="font-size:18px;">package cn.itcast.purchasing.util;  
  
import javax.servlet.http.HttpServletRequest;  
  
import org.springframework.context.ApplicationContext;  
import org.springframework.web.context.request.RequestContextHolder;  
import org.springframework.web.context.request.ServletRequestAttributes;  
import org.springframework.web.context.support.WebApplicationContextUtils;  
  
public class ApplicationContextUtils {  
      
    private static ApplicationContext applicationContext;  
      
    public static ApplicationContext getApplicationContext(){  
          
        if(applicationContext == null){  
            HttpServletRequest request = ((ServletRequestAttributes)RequestContextHolder.getRequestAttributes()).getRequest();  
            applicationContext = WebApplicationContextUtils.getRequiredWebApplicationContext(request.getServletContext());   
              
        }  
        return applicationContext;  
    }  
}</span>  

应用实战代码：
[java] view plain copy
<span style="font-size:18px;">//通过工具类获取spring容器  
private static ApplicationContext applicationContext = ApplicationContextUtils.getApplicationContext();  
@Override  
public void notify(DelegateExecution execution) throws Exception {  
    //从spring容器中得到mapper  
    PurBusOrderMapper purBusOrderMapper = (PurBusOrderMapper) applicationContext.getBean("purBusOrderMapper");  
    purBusOrderMapper.updateByPrimaryKeySelective("要更新的实体对象");  
}</span>  
