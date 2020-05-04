# 枚举的自定义值使用方法

\*\*\*\*

```java
/**
 * @author zhao xubin
 * @date 2020-3-28
 */
public enum ContentType {

    JSON("application/json");

    private String value;

    ContentType(String value) {
        this.value = value;
    }

    public String getValue() {
        return this.value;
    }
}
```

