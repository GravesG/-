> @ConfigurationProperties

@ConfigurationProperties（prefix = ”person“）

可以通过此注解关联对象



> @PropertySource

加载指定的配置文件

@PropertySource(value = "classpath:****.properties")



@Value("${name}")        配合SPEL表达式







> SpringBoot自动装配

启动时 会加载 autoConfig包下的 MATE-INF/spring.factories，里面配置了大量的配置类（****AutoConfiguration）， 每个对应的AutoConfiguration  又有一个 对应的 ****Properties



> @Condition

该注解判断存在某个条件时，才生效某某配置





> SpringBoot配置文件的位置 及 静态资源 的位置和优先级



> i18n  国际化

messageSourceAutoConfiguration

自定义组件LocaleResolver

i18n 放在resourse 目录下即可



> SpringBoot 的三大特性

- 组件自动装配
- 嵌入式Web容器
- 生产准备指标



> 组件自动装配

- 激活：@EnableAutoConfiguration
- 配置：/MATE-INF/spring.factories
- 实现：XXXAutoConfiguration



