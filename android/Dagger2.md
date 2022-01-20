# Dagger2

## @Inject
针对成员变量：
```java
public class SamsungPhone implements IPhone {
    @Inject
    ICamera camera;

    public SamsungPhone() {
    }

    public String name() {
        return "SamsungPhone";
    }
}
```
```java
@DaggerGenerated
public final class SamsungPhone_MembersInjector implements MembersInjector<SamsungPhone> {
    private final Provider<ICamera> cameraProvider;

    public SamsungPhone_MembersInjector(Provider<ICamera> cameraProvider) {
        this.cameraProvider = cameraProvider;
    }

    public static MembersInjector<SamsungPhone> create(Provider<ICamera> cameraProvider) {
        return new SamsungPhone_MembersInjector(cameraProvider);
    }

    public void injectMembers(SamsungPhone instance) {
        injectCamera(instance, (ICamera)this.cameraProvider.get());
    }

    @InjectedFieldSignature("com.charlie.dagger2.phone.SamsungPhone.camera")
    public static void injectCamera(SamsungPhone instance, ICamera camera) {
        instance.camera = camera;
    }
}
```

针对构造方法：
```java
public class SamsungCamera implements ICamera {
    @Inject
    public SamsungCamera() {
    }

    public String name() {
        return "SamsungCamera";
    }
}
```
```java
@DaggerGenerated
public final class SamsungCamera_Factory implements Factory<SamsungCamera> {
    public SamsungCamera_Factory() {
    }

    public SamsungCamera get() {
        return newInstance();
    }

    public static SamsungCamera_Factory create() {
        return SamsungCamera_Factory.InstanceHolder.INSTANCE;
    }

    public static SamsungCamera newInstance() {
        return new SamsungCamera();
    }

    private static final class InstanceHolder {
        private static final SamsungCamera_Factory INSTANCE = new SamsungCamera_Factory();

        private InstanceHolder() {
        }
    }
}
```

## @Component
```java
@Component
public interface PhoneComponent {
}
```
```java
@DaggerGenerated
public final class DaggerPhoneComponent implements PhoneComponent {
    private final DaggerPhoneComponent phoneComponent;

    private DaggerPhoneComponent() {
        this.phoneComponent = this;
    }

    public static DaggerPhoneComponent.Builder builder() {
        return new DaggerPhoneComponent.Builder();
    }

    public static PhoneComponent create() {
        return (new DaggerPhoneComponent.Builder()).build();
    }

    public static final class Builder {
        private Builder() {
        }

        public PhoneComponent build() {
            return new DaggerPhoneComponent();
        }
    }
}
```