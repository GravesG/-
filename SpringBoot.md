> @ConfigurationProperties

@ConfigurationProperties（prefix = ”person“）

可以通过此注解关联对象



> @PropertySource

加载指定的配置文件

@PropertySource(value = "classpath:****.properties")



@Value("${name}")        配合SPEL表达式