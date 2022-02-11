# springboot 实现验证码

**1、kaptcha简介**

kaptcha 是一个非常实用的验证码生成工具。有了它，你可以生成各种样式的验证码，因为它是可配置的。kaptcha工作的原理是调用 com.google.code.kaptcha.servlet.KaptchaServlet，生成一个图片。同时将生成的验证码字符串放到 HttpSession中。

**使用kaptcha可以方便的配置：**

    验证码的字体
    验证码字体的大小
    验证码字体的字体颜色
    验证码内容的范围(数字，字母，中文汉字！)
    验证码图片的大小，边框，边框粗细，边框颜色
    验证码的干扰线(可以自己继承com.google.code.kaptcha.NoiseProducer写一个自定义的干扰线)
    验证码的样式(鱼眼样式、3D、普通模糊……当然也可以继承com.google.code.kaptcha.GimpyEngine自定义样式)

spring boot集成kaptcha案例
**新建spring boot maven工程并引入kaptcha依赖**

```java
    <!-- 验证码 依赖-->
    <dependency>
        <groupId>com.github.axet</groupId>
        <artifactId>kaptcha</artifactId>
        <version>0.0.9</version>
    </dependency>
```

**配置Kaptcha**

~~~java
import com.google.code.kaptcha.impl.DefaultKaptcha;
import com.google.code.kaptcha.util.Config;
import org.springframework.context.annotation.Bean;
import org.springframework.stereotype.Component;

import java.util.Properties;

/**
 * 验证码配置类
 */
@Component
public class KaptchaConfig {

    @Bean
    public DefaultKaptcha getDefaultKaptcha() {
        com.google.code.kaptcha.impl.DefaultKaptcha defaultKaptcha = new com.google.code.kaptcha.impl.DefaultKaptcha();
        Properties properties = new Properties();
        // 图片边框
        properties.setProperty("kaptcha.border", "no");
        // 边框颜色
        properties.setProperty("kaptcha.border.color", "black");
        //边框厚度
        properties.setProperty("kaptcha.border.thickness", "1");
        // 图片宽
        properties.setProperty("kaptcha.image.width", "200");
        // 图片高
        properties.setProperty("kaptcha.image.height", "50");
        //图片实现类
        properties.setProperty("kaptcha.producer.impl", "com.google.code.kaptcha.impl.DefaultKaptcha");
        //文本实现类
        properties.setProperty("kaptcha.textproducer.impl", "com.google.code.kaptcha.text.impl.DefaultTextCreator");
        //文本集合，验证码值从此集合中获取
        properties.setProperty("kaptcha.textproducer.char.string", "01234567890qwertyuiopasdfghjklzxcvbnm");
        //验证码长度
        properties.setProperty("kaptcha.textproducer.char.length", "4");
        //字体
        properties.setProperty("kaptcha.textproducer.font.names", "宋体");
        //字体颜色
        properties.setProperty("kaptcha.textproducer.font.color", "black");
        //文字间隔
        properties.setProperty("kaptcha.textproducer.char.space", "5");
        //干扰实现类
        properties.setProperty("kaptcha.noise.impl", "com.google.code.kaptcha.impl.DefaultNoise");
        //干扰颜色
        properties.setProperty("kaptcha.noise.color", "blue");
        //干扰图片样式
        properties.setProperty("kaptcha.obscurificator.impl", "com.google.code.kaptcha.impl.WaterRipple");
        //背景实现类
        properties.setProperty("kaptcha.background.impl", "com.google.code.kaptcha.impl.DefaultBackground");
        //背景颜色渐变，结束颜色
        properties.setProperty("kaptcha.background.clear.to", "white");
        //文字渲染器
        properties.setProperty("kaptcha.word.impl", "com.google.code.kaptcha.text.impl.DefaultWordRenderer");
        Config config = new Config(properties);
        defaultKaptcha.setConfig(config);
        return defaultKaptcha;
    }
}

~~~

#### 生成验证码的Controller

~~~java

/**
 * 验证码Controller
 * 主要生成验证码
 * 获取验证码 String code = (String)request.getSession().getAttribute(Constants.KAPTCHA_SESSION_KEY);
 *
 */
@Controller
@RequestMapping("/kaptcha")
public class CaptchaController {

    @Autowired
    private Producer captchaProducer;

    @Autowired
    private static Logger logger = LoggerFactory.getLogger(CaptchaController.class);


    @RequestMapping("getKaptchaImage")
    public ModelAndView getKaptchaImage(HttpServletRequest request, HttpServletResponse response) throws Exception {
        HttpSession session = request.getSession();
        String code = (String)session.getAttribute(Constants.KAPTCHA_SESSION_KEY);
        logger.debug("******************验证码是: " + code + "******************");

        response.setDateHeader("Expires", 0);

        // Set standard HTTP/1.1 no-cache headers.
        response.setHeader("Cache-Control", "no-store, no-cache, must-revalidate");

        // Set IE extended HTTP/1.1 no-cache headers (use addHeader).
        response.addHeader("Cache-Control", "post-check=0, pre-check=0");

        // Set standard HTTP/1.0 no-cache header.
        response.setHeader("Pragma", "no-cache");

        // return a jpeg
        response.setContentType("image/jpeg");

        // create the text for the image
        String capText = captchaProducer.createText();

        // store the text in the session
        session.setAttribute(Constants.KAPTCHA_SESSION_KEY, capText);

        // create the image with the text
        BufferedImage bi = captchaProducer.createImage(capText);
        ServletOutputStream out = response.getOutputStream();

        // write the data out
        ImageIO.write(bi, "jpg", out);
        try {
            out.flush();
        } finally {
            out.close();
        }
        return null;
    }
}


~~~

### 后台登录Controller

~~~java
@RestController
public class LoginController {


    @GetMapping("/login")
    public String login(HttpServletRequest request, @RequestParam("code")String code){


        String sessionCode = (String)request.getSession().getAttribute(Constants.KAPTCHA_SESSION_KEY);
        System.out.println(" session中获取的验证码:  " + sessionCode);
        if (code.equalsIgnoreCase(sessionCode)){
            return "验证码相同,验证通过";
        }else {
            return "验证码不同,验证失败";
        }

    }
}

~~~

#### 创建前台html页面login.html

~~~java
<!DOCTYPE html>
<html>

<head>

    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <title>INSPINIA | Login 2</title>

    <link rel="stylesheet" href="https://cdn.staticfile.org/twitter-bootstrap/4.3.1/css/bootstrap.min.css">
    <script src="https://cdn.staticfile.org/jquery/3.2.1/jquery.min.js"></script>
    <script src="https://cdn.staticfile.org/twitter-bootstrap/4.3.1/js/bootstrap.min.js"></script>
</head>

<body class="gray-bg">

<div class="loginColumns animated fadeInDown">
    <div class="row">

        <div class="col-md-6">
            <div class="ibox-content">
                <form class="m-t" role="form" action="http://127.0.0.1:8080/login">
                    <div class="form-group">
                        <input type="text" class="form-control" placeholder="用户名" required="">
                    </div>
                    <div class="form-group">
                        <input type="password" class="form-control" placeholder="密码" required="">
                    </div>
                    <div class="form-group">
                        <input type="text" class="form-control" name="code" placeholder="验证码" required="">
                        <span class="p-new-code l-mar-r15"> <img src="http://127.0.0.1:8080/kaptcha/getKaptchaImage" class="reloadImage" id="reloadImage"
                                                                 width="121" height="40"/> </span>
                        <a href="javaScript:;" class="l-color9 reloadImage">看不清楚，换一张</a>
                    </div>
                    <button  id = "login" type="submit" class="btn btn-primary block full-width m-b">Login</button>

                </form>
            </div>
        </div>
    </div>
    <hr/>
   
</div>

</body>
<script>
    $(".reloadImage").click(function () {
        //获取当前的时间作为参数，无具体意义
        var timenow = new Date().getTime();
        $('#reloadImage').attr("src", "http://127.0.0.1:8080/kaptcha/getKaptchaImage?date=" + timenow);
    });

</script>

</html>


~~~

