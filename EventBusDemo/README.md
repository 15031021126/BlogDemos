## EventBus�Ļ���ʹ�ú�Դ�����
[���͵�ַ](http://shenhuniurou.com/2016/11/06/android-event-bus-source-parsing)

�������Ŀ��ʹ����EventBus(3.0)�����÷ǳ����ã����ǾͿ���һЩ����EventBusԴ����������£����ڼ�¼�����Ļ���ʹ�÷����Ϳ�Դ������з�����һЩ���⡣[EventBusԴ���ַ](https://github.com/greenrobot/EventBus)


![11212.gif](http://upload-images.jianshu.io/upload_images/1159224-bee2ae64e82d2340.gif?imageMogr2/auto-orient/strip)


## EventBus��ʲô��

EventBus��һ��Android�¼������Ͷ��ĵĿ�ܣ�ͨ��������ߺͶ���������Android�¼����ݡ�

## EventBus�Ǹ�ʲô�ģ�


![EventBus-Publish-Subscribe.png](http://upload-images.jianshu.io/upload_images/1159224-bb2f6dfa7493b635.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


�¼����ݼȿ�������Android�Ĵ������ͨѶ��Ҳ���������첽�̺߳����̼߳�ͨѶ�ȡ���ͳ���¼����ݷ�ʽ������`Handler`��`BroadcastReceiver`��`Interface�ص�`����EventBus�������������¼��ģ���ȴ�ͳ�ķ�ʽ���������࣬ʹ�ü򵥣������¼������Ͷ��ĳ�ֽ��

## ����ʹ�÷���

**���������**

```java
//��app�µ�build.gradle��������������
compile 'org.greenrobot:eventbus:3.0.0'
```

**ע�ᶩ���¼�**

```java
@Override
protected void onCreate(Bundle savedInstanceState) {
	super.onCreate(savedInstanceState);
	setContentView(R.layout.activity_main);

	EventBus.getDefault().register(this);
}
```

**ȡ��ע�ᶩ���¼�**

```java
@Override
protected void onDestroy() {
	super.onDestroy();
	EventBus.getDefault().unregister(this);
}
```

**�����¼�**
```java
EventBus.getDefault().post(Object obj);
```

**�����¼�**
```java
@Subscribe(threadMode = ThreadMode.MAIN)
public void onEvent(Object obj) {
}
```

���ɣ�ʹ����������ͦ�򵥵ģ�������������Ҫ�����߼��ľ�������ע��@subscribe�ķ���`onEvent(Object obj)`��ȥ���������Լ�������ȥ������Ȼ���﷽������һ����`onEvent`�ˣ�������2.4�汾Ӱ�죬ϰ���˰ѷ�����д��`onEvent`����3.0�汾�п�������д�����ǲ�����`class`���ͱ���Ҫ��������`post`�Ĳ�����`class`����һ�¡�

**ʹ�ó���**

������һ���б�ҳ�棬ÿ��item�϶������������������͵���״̬�����ȥ������ҳ���û����Խ���һЩ���������ۣ����ޣ���ʱ����ϸҳ���������������͵���״̬���ǿ����������£����ǵ����ǻص��б�ҳ���ʱ��Ҳ��Ҫ����UI����֮ǰҲ˵�ˣ���ͳ���¼����ݷ�ʽ��Handler��BroadcastReceiver���Լ�Interface�ص��ȣ������ʹ��EventBus�Ļ����һῼ��ʹ�ù㲥�ķ�ʽ������`startActivityForResult`������ϸҳ���ȵ�����ϸҳ���ص��б�ҳʱ����������Ķ��󴫵ݻ�����Ȼ����`onActivityResult`������ȥ�������UI���������ַ����ı׶�����ֻ���ڷ��ص���һ������ʱ�ŻὫ�¼����ݻ��������ܴﵽ�������µ�Ŀ�ġ�

**�߳�ģʽ**

����˵����ֻ��ʹ��ע��`@Subscribe(threadMode = ThreadMode.MAIN)`�����ķ������Ƕ�������Ҫ�����߼��ĵط��������`threadMode `��ʲô�أ�����ʵ��ָ�������ߴ����¼����̣߳�EventBus�ܹ��������߳�ģʽ���ֱ��ǣ�

- ThreadMode.MAIN����ʾ�����¼������ĸ��̷߳��������ģ����¼����ķ���onEvent������UI�߳���ִ�У������Android���Ƿǳ����õģ���Ϊ��Android��ֻ����UI�߳��и���UI�������ڴ�ģʽ�µķ����ǲ���ִ�к�ʱ�����ġ�

- ThreadMode.POSTING����ʾ�¼����ĸ��߳��з��������ģ��¼����ĺ���onEvent�ͻ�������߳������У�Ҳ����˵�����¼��ͽ����¼���ͬһ���̡߳�ʹ���������ʱ����onEvent�����в���ִ�к�ʱ���������ִ�к�ʱ�������׵����¼��ַ��ӳ١�

- ThreadMode.BACKGROUND����ʾ����¼���UI�߳��з��������ģ���ô���ĺ���onEvent�ͻ������߳������У�����¼��������������߳��з��������ģ���ô���ĺ���ֱ���ڸ����߳���ִ�С�

- ThreadMode.ASYNC��ʹ�����ģʽ�Ķ��ĺ�������ô�����¼����ĸ��̷߳��������ᴴ���µ����߳���ִ�ж��ĺ�����

> ASYNC���ǰ���߲�ͬ�ĵط��ǿ��Դ����ʱ�Ĳ�������������̳߳أ�����һ���첽ִ�еĹ��̣����¼��Ķ����߿��������õ�ִ�С���ȻBACKGROUNDҲ�������̳߳أ�����ÿ��ֻ��ִ��һ�����񣬾��ǲ����첽ִ�С�����һ���Ǹ���UI���¼�����ʹ��ThreadMode.MAIN����Ҫ����������¼���ʹ��ThreadMode.AYSNC,��

```java
final class BackgroundPoster implements Runnable {

    private final PendingPostQueue queue;
    private final EventBus eventBus;

    private volatile boolean executorRunning;

    BackgroundPoster(EventBus eventBus) {
        this.eventBus = eventBus;
        queue = new PendingPostQueue();
    }

    public void enqueue(Subscription subscription, Object event) {
        PendingPost pendingPost = PendingPost.obtainPendingPost(subscription, event);
        synchronized (this) {
            queue.enqueue(pendingPost);
            if (!executorRunning) {
                executorRunning = true;
                eventBus.getExecutorService().execute(this);
            }
        }
    }

    @Override
    public void run() {
        try {
            try {
                while (true) {
                    PendingPost pendingPost = queue.poll(1000);
                    if (pendingPost == null) {
                        synchronized (this) {
                            // Check again, this time in synchronized
                            pendingPost = queue.poll();
                            if (pendingPost == null) {
                                executorRunning = false;
                                return;
                            }
                        }
                    }
                    eventBus.invokeSubscriber(pendingPost);
                }
            } catch (InterruptedException e) {
                Log.w("Event", Thread.currentThread().getName() + " was interruppted", e);
            }
        } finally {
            executorRunning = false;
        }
    }

}
```

backgroundPosterͨ��enqueue����������ǰ�Ķ��������������PendingPostQueue
�У��Ƿ�����ִ�У�����Ҫ�жϵ�ǰ�����Ƿ�������ִ�е�������û�еĻ���������ִ�У�������ִ������Ļ�����ֻ���ж��е���ӡ���������֤�˺�̨������Զֻ����һ���߳�ִ�С�

```java
class AsyncPoster implements Runnable {

    private final PendingPostQueue queue;
    private final EventBus eventBus;

    AsyncPoster(EventBus eventBus) {
        this.eventBus = eventBus;
        queue = new PendingPostQueue();
    }

    public void enqueue(Subscription subscription, Object event) {
        PendingPost pendingPost = PendingPost.obtainPendingPost(subscription, event);
        queue.enqueue(pendingPost);
        eventBus.getExecutorService().execute(this);
    }

    @Override
    public void run() {
        PendingPost pendingPost = queue.poll();
        if(pendingPost == null) {
            throw new IllegalStateException("No pending post available");
        }
        eventBus.invokeSubscriber(pendingPost);
    }

}
```

AsyncPoster��ֱ��ͨ���̳߳ص���ִ�У����BackgroundPosterִ����˵����û�еȴ��Ĺ��̡�

```java
final class HandlerPoster extends Handler {

    private final PendingPostQueue queue;
    private final int maxMillisInsideHandleMessage;
    private final EventBus eventBus;
    private boolean handlerActive;

    HandlerPoster(EventBus eventBus, Looper looper, int maxMillisInsideHandleMessage) {
        super(looper);
        this.eventBus = eventBus;
        this.maxMillisInsideHandleMessage = maxMillisInsideHandleMessage;
        queue = new PendingPostQueue();
    }

    void enqueue(Subscription subscription, Object event) {
        PendingPost pendingPost = PendingPost.obtainPendingPost(subscription, event);
        synchronized (this) {
            queue.enqueue(pendingPost);
            if (!handlerActive) {
                handlerActive = true;
                if (!sendMessage(obtainMessage())) {
                    throw new EventBusException("Could not send handler message");
                }
            }
        }
    }

    @Override
    public void handleMessage(Message msg) {
        boolean rescheduled = false;
        try {
            long started = SystemClock.uptimeMillis();
            while (true) {
                PendingPost pendingPost = queue.poll();
                if (pendingPost == null) {
                    synchronized (this) {
                        // Check again, this time in synchronized
                        pendingPost = queue.poll();
                        if (pendingPost == null) {
                            handlerActive = false;
                            return;
                        }
                    }
                }
                eventBus.invokeSubscriber(pendingPost);
                long timeInMethod = SystemClock.uptimeMillis() - started;
                if (timeInMethod >= maxMillisInsideHandleMessage) {
                    if (!sendMessage(obtainMessage())) {
                        throw new EventBusException("Could not send handler message");
                    }
                    rescheduled = true;
                    return;
                }
            }
        } finally {
            handlerActive = rescheduled;
        }
    }
}
```

HandlerPoster����������Ϥ������Handler��Ϣ�����ˣ����Ὣÿ�������¼����ݵ�UI�߳��н��д�������Ͳ���˵�ˡ�

## Դ�����

**EventBus���¼�����**

��EventBus�У������Ķ��Ķ�����`SubscriberMethod`����������Ӧ��`Method��`���Լ��¼���������`Class<?> eventType`�����������̣߳����ȼ����Ƿ�`Sticky`��Ϣ��

```java
public class SubscriberMethod {
    final Method method;
    final ThreadMode threadMode;
    final Class<?> eventType;
    final int priority;
    final boolean sticky;
}
```

> Sticky=true��ʾ�ȵ��ص��¼����ĵĽ���ʱ�ſ�ʼ��������������һpost�Ϳ�ʼ���ݣ�ʲôʱ��ʹ��sticy�أ�����ϣ������¼��������ϴ����ʱ��

�¼����ߣ�һ�㶼��Ӧ��һ�����ϣ���������еĶ�����Ƕ��ĵ��¼�����EventBus��ʹ�õ��ǣ�

```java
private final Map<Class<?>, CopyOnWriteArrayList<Subscription>> subscriptionsByEventType;

subscriptionsByEventType = new HashMap<>();
```

�����Map���ϲ��õ���һ��HashMap���ϣ�map��key��Ӧ����֮ǰ`SubscriberMethod`�е�`eventType`, value���Ӧ��һ���̰߳�ȫ��List��List�д�ŵ��ǰ������Ķ���`Object`����Ӧ���ķ���`SubscriberMethod`��`Subscription`�ࣺ

```java
final class Subscription {
    final Object subscriber;
    final SubscriberMethod subscriberMethod;
}
```

���ԣ�EventBus�ڲ�������һ���̰߳�ȫ��List������ά�����еĶ����ߣ����¼����߼��ϡ���ע�ᶩ�ĺ�ȡ��ע�ᶩ�ľ��Ƕ����List���Ͻ�����ɾ�Ĺ��̡�

**ע�ᶩ�ĺ�ȡ��ע�ᶩ��**

```java
/**
 * Registers the given subscriber to receive events. Subscribers must call {@link #unregister(Object)} once they
 * are no longer interested in receiving events.
 * <p/>
 * Subscribers have event handling methods that must be annotated by {@link Subscribe}.
 * The {@link Subscribe} annotation also allows configuration like {@link
 * ThreadMode} and priority.
 */
public void register(Object subscriber) {
	Class<?> subscriberClass = subscriber.getClass();
	List<SubscriberMethod> subscriberMethods = subscriberMethodFinder.findSubscriberMethods(subscriberClass);
	synchronized (this) {
		for (SubscriberMethod subscriberMethod : subscriberMethods) {
			subscribe(subscriber, subscriberMethod);
		}
	}
}
```

����EventBus����ݶ������Classȥ������в��Ҵ������¼��ķ�����`SubscriberMethodFinder`����࿴�������Ǿ�֪�����Ǹ�ʲô�õ��ˣ��Ȼ���ϸ˵���������ǿ���`subscribe`������

```java
// Must be called in synchronized block
private void subscribe(Object subscriber, SubscriberMethod subscriberMethod) {
	Class<?> eventType = subscriberMethod.eventType;
	Subscription newSubscription = new Subscription(subscriber, subscriberMethod);
	CopyOnWriteArrayList<Subscription> subscriptions = subscriptionsByEventType.get(eventType);
	if (subscriptions == null) {
		subscriptions = new CopyOnWriteArrayList<>();
		subscriptionsByEventType.put(eventType, subscriptions);
	} else {
		if (subscriptions.contains(newSubscription)) {
			throw new EventBusException("Subscriber " + subscriber.getClass() + " already registered to event "
					+ eventType);
		}
	}

	int size = subscriptions.size();
	for (int i = 0; i <= size; i++) {
		if (i == size || subscriberMethod.priority > subscriptions.get(i).subscriberMethod.priority) {
			subscriptions.add(i, newSubscription);
			break;
		}
	}

	List<Class<?>> subscribedEvents = typesBySubscriber.get(subscriber);
	if (subscribedEvents == null) {
		subscribedEvents = new ArrayList<>();
		typesBySubscriber.put(subscriber, subscribedEvents);
	}
	subscribedEvents.add(eventType);

	if (subscriberMethod.sticky) {
		if (eventInheritance) {
			// Existing sticky events of all subclasses of eventType have to be considered.
			// Note: Iterating over all events may be inefficient with lots of sticky events,
			// thus data structure should be changed to allow a more efficient lookup
			// (e.g. an additional map storing sub classes of super classes: Class -> List<Class>).
			Set<Map.Entry<Class<?>, Object>> entries = stickyEvents.entrySet();
			for (Map.Entry<Class<?>, Object> entry : entries) {
				Class<?> candidateEventType = entry.getKey();
				if (eventType.isAssignableFrom(candidateEventType)) {
					Object stickyEvent = entry.getValue();
					checkPostStickyEventToSubscription(newSubscription, stickyEvent);
				}
			}
		} else {
			Object stickyEvent = stickyEvents.get(eventType);
			checkPostStickyEventToSubscription(newSubscription, stickyEvent);
		}
	}
}
```

������������ǿ��Կ���ÿһ��`eventType` ����Ӧһ��`CopyOnWriteArrayList<Subscription>`������������Դ��̫��������ˣ������Demo�е�������˵��`MainActivity`�е�`subscriberMethod`����`onEvent(NewsModel newsModel)`����������`eventType`����`NewsModel`�࣬��`subscriber`����MainActivity��ʵ����������ϳ���һ��������`Subscription`��Ȼ���`eventType`��Ϊkey��������`NewsModel`�����¼��Ķ����ߵļ�����Ϊvalue�洢��һ��HashMap�У����������ȼ�`priority `��`newSubscription `������µĶ�������ӵ������б��С�Ҳ����˵��һ��Activityҳ���࣬���ǿ����ж��SubscriberMethod����������������

```java
@Subscribe(threadMode = ThreadMode.MAIN)
public void onEvent(NewsModel newsModel) {
	newsList.set(clickPosition, newsModel);
	mAdapter.notifyItemChanged(clickPosition);
}

@Subscribe(threadMode = ThreadMode.BACKGROUND)
public void onEvent1(User user) {
	
}

@Subscribe(threadMode = ThreadMode.ASYNC)
public void onEvent2(Person person) {
	
}
```

ȡ��ע�ᶩ�����ǽ����¼����͵Ķ����ߴ��¼����߼������Ƴ���

```java
/** Unregisters the given subscriber from all event classes. */
public synchronized void unregister(Object subscriber) {
	List<Class<?>> subscribedTypes = typesBySubscriber.get(subscriber);
	if (subscribedTypes != null) {
		for (Class<?> eventType : subscribedTypes) {
			unsubscribeByEventType(subscriber, eventType);
		}
		typesBySubscriber.remove(subscriber);
	} else {
		Log.w(TAG, "Subscriber to unregister was not registered before: " + subscriber.getClass());
	}
}

/** Only updates subscriptionsByEventType, not typesBySubscriber! Caller must update typesBySubscriber. */
private void unsubscribeByEventType(Object subscriber, Class<?> eventType) {
	List<Subscription> subscriptions = subscriptionsByEventType.get(eventType);
	if (subscriptions != null) {
		int size = subscriptions.size();
		for (int i = 0; i < size; i++) {
			Subscription subscription = subscriptions.get(i);
			if (subscription.subscriber == subscriber) {
				subscription.active = false;
				subscriptions.remove(i);
				i--;
				size--;
			}
		}
	}
}
```

**�¼����Ѻͷ����¼�**

����ͨ��EventBus��post(Object event)�����������¼��ķ�����������EventBus������List���ҳ����������event�ķ���Subscription��Ȼ�����methodָ���Ĳ�ͬ�߳���Ϣ������������ĵ��ã���������Ӧ�߳��е��ã�����EventBus�е�`post`������

```java
/** Posts the given event to the event bus. */
public void post(Object event) {
	PostingThreadState postingState = currentPostingThreadState.get();
	List<Object> eventQueue = postingState.eventQueue;
	eventQueue.add(event);

	if (!postingState.isPosting) {
		postingState.isMainThread = Looper.getMainLooper() == Looper.myLooper();
		postingState.isPosting = true;
		if (postingState.canceled) {
			throw new EventBusException("Internal error. Abort state was not reset");
		}
		try {
			while (!eventQueue.isEmpty()) {
				postSingleEvent(eventQueue.remove(0), postingState);
			}
		} finally {
			postingState.isPosting = false;
			postingState.isMainThread = false;
		}
	}
}
```

˳��`postSingleEvent`���������������ǻᷢ�����ս�����÷���`postToSubscription`�������¼����ݵ���������������

```java
private void postToSubscription(Subscription subscription, Object event, boolean isMainThread) {
	switch (subscription.subscriberMethod.threadMode) {
		case POSTING:
			invokeSubscriber(subscription, event);
			break;
		case MAIN:
			if (isMainThread) {
				invokeSubscriber(subscription, event);
			} else {
				mainThreadPoster.enqueue(subscription, event);
			}
			break;
		case BACKGROUND:
			if (isMainThread) {
				backgroundPoster.enqueue(subscription, event);
			} else {
				invokeSubscriber(subscription, event);
			}
			break;
		case ASYNC:
			asyncPoster.enqueue(subscription, event);
			break;
		default:
			throw new IllegalStateException("Unknown thread mode: " + subscription.subscriberMethod.threadMode);
	}
}
```

```java
void invokeSubscriber(Subscription subscription, Object event) {
	try {
		subscription.subscriberMethod.method.invoke(subscription.subscriber, event);
	} catch (InvocationTargetException e) {
		handleSubscriberException(subscription, event, e.getCause());
	} catch (IllegalAccessException e) {
		throw new IllegalStateException("Unexpected exception", e);
	}
}
```

Ȼ��ͨ��������ö����ߵ����Ѹ��¼��ķ�����Ҳ���㿴����`postToSubscription`����ʱ�ᷢ��`threadMode`��`MAIN`��`BACKGROUND`����`ASYNC`ʱ�������ø����سǵ�poster����`enqueue`��������ʵ��ϸһ��㿴��ȥ��ᷢ�ֵ�����ǵ�����`invokeSubscriber`���������ԣ����յ����ѷ������þ������д��룺

```java
subscription.subscriberMethod.method.invoke(subscription.subscriber, event);
```

�������Ѿ�����װ�طǳ�������������������ʹ�ò�ͬ���̵߳��Ȳ��Ծͺܼ��ˣ�����ָ��һ��`ThreadMode`��������ָ���߳��е��á�

�������Ҳ����ᷢ���и��ܴ�����ʣ�`SubscriberMethod`��Ϣ����ô���ɵģ���ʵ���Ǽ���ע��`@Subscribe`�ķ��������ǿ���ͨ��������ʱ���÷���ķ�������ȡ��Ӧ�����ע��ķ������ٷ�װ��Ϊ`SubscriberMethod`��Ҳ����ǰ�����ᵽ��`SubscriberMethodFinder`������ȡ�����¼���������Դ�룬����������ʱ���÷���ȥ����`SubscriberMethod`�Ĵ��룺

```java
private void findUsingReflectionInSingleClass(FindState findState) {
	Method[] methods;
	try {
		// This is faster than getMethods, especially when subscribers are fat classes like Activities
		methods = findState.clazz.getDeclaredMethods();
	} catch (Throwable th) {
		// Workaround for java.lang.NoClassDefFoundError, see https://github.com/greenrobot/EventBus/issues/149
		methods = findState.clazz.getMethods();
		findState.skipSuperClasses = true;
	}
	for (Method method : methods) {
		int modifiers = method.getModifiers();
		if ((modifiers & Modifier.PUBLIC) != 0 && (modifiers & MODIFIERS_IGNORE) == 0) {
			Class<?>[] parameterTypes = method.getParameterTypes();
			if (parameterTypes.length == 1) {
				Subscribe subscribeAnnotation = method.getAnnotation(Subscribe.class);
				if (subscribeAnnotation != null) {
					Class<?> eventType = parameterTypes[0];
					if (findState.checkAdd(method, eventType)) {
						ThreadMode threadMode = subscribeAnnotation.threadMode();
						findState.subscriberMethods.add(new SubscriberMethod(method, eventType, threadMode,
								subscribeAnnotation.priority(), subscribeAnnotation.sticky()));
					}
				}
			} else if (strictMethodVerification && method.isAnnotationPresent(Subscribe.class)) {
				String methodName = method.getDeclaringClass().getName() + "." + method.getName();
				throw new EventBusException("@Subscribe method " + methodName +
						"must have exactly 1 parameter but has " + parameterTypes.length);
			}
		} else if (strictMethodVerification && method.isAnnotationPresent(Subscribe.class)) {
			String methodName = method.getDeclaringClass().getName() + "." + method.getName();
			throw new EventBusException(methodName +
					" is a illegal @Subscribe method: must be public, non-static, and non-abstract");
		}
	}
}
```

�������ǿ�������˵��ʹ�÷���������������ģ��Ǳ���Ҫ���ǵ��ġ���3.0�İ汾��`EventBus`������`apt`������߼����и�[Subscriber Index](http://greenrobot.org/eventbus/documentation/subscriber-index/)�Ľ��ܣ���Ҫ��ͨ��Apt�ڱ����ڸ���ע��ֱ��������Ӧ����Ϣ��������������ʱͨ����������ȡ��ʹ�÷������£�

```java
buildscript {
    dependencies {
        classpath 'com.neenbedankt.gradle.plugins:android-apt:1.8'
    }
}
 
apply plugin: 'com.neenbedankt.android-apt'
 
dependencies {
    compile 'org.greenrobot:eventbus:3.0.0'
    apt 'org.greenrobot:eventbus-annotation-processor:3.0.1'
}
 
apt {
    arguments {
        eventBusIndex "com.example.myapp.MyEventBusIndex"
    }
}
```


![11211.png](http://upload-images.jianshu.io/upload_images/1159224-70e79ea8ba9b016a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

��������ó��˵�һ������project��build.gradle����ӣ�����Ķ�����moudle�е�build.gradle����ӣ�����Ĵ�ҿ��Կ��ҵ�Demo�����÷������������ٴα���֮��ϵͳ���Զ�Ϊ��������`MyEventBusIndex`����࣬Ȼ�����ǽ������ø�EventBus��

```java
EventBus eventBus = EventBus.builder().addIndex(new MyEventBusIndex()).build();
```

������������

```java
EventBus.builder().addIndex(new MyEventBusIndex()).installDefaultEventBus();
// Now the default instance uses the given index. Use it like this:
EventBus eventBus = EventBus.getDefault();
```

`MyEventBusIndex`������ɵ�������ʵ���ϻ�ʵ��`SubscriberInfoIndex`����ӿڣ�

```java
public interface SubscriberInfoIndex {
    SubscriberInfo getSubscriberInfo(Class<?> subscriberClass);
}
```

�ӽӿ��п��Կ��������Index��ֻ�ṩ��һ������`getSubsriberInfo`�����������Ҫ���Ǵ��붩�������ڵ�class�࣬Ȼ����һ��`SubscriberInfo`���࣬����ṹ���£�

```java
public interface SubscriberInfo {
    Class<?> getSubscriberClass();

    SubscriberMethod[] getSubscriberMethods();

    SubscriberInfo getSuperSubscriberInfo();

    boolean shouldCheckSuperclass();
}
```

�ӽӿ��еķ������ɻ�ȡ���������`SubscriberMethod`��Ϣ������ֻ��һ����ȡ������ķ�������apt���ɵ�`MyEventBusIndex`�У��Ὣ���е���Щclass��`SubscriberInfo`�����ھ�̬����`hashMap`�ṹ�������ʹﵽ�˱��������ڷ����ȡ���ɶ��ķ������������⡣���������һ�����̣���������ע���ʱ��ֱ��ͨ��`HashMap`�еĶ����࣬��ȡ��`SubscriberInfo`��������ȡ�����е�`SubscerberMethod`������װΪ`Subscription`������ӵ��¼������С���������������apt֮��EventBus����������͵��Խ���ˡ�

˵�����������⣬���ý���˵`SubscriberMethodFinder`����࣬������ݶ�����class��Ϣ������ȡ`SubscriberMethod`��EventBus�ṩ�����ַ�ʽ���л�ȡ��

```java
if (ignoreGeneratedIndex) {
	subscriberMethods = findUsingReflection(subscriberClass);
} else {
	subscriberMethods = findUsingInfo(subscriberClass);
}
```

- �����ʹ�����ɵ����������ң��Ͳ���`findUsingReflection(subscriberClass)`�������з�������ȡ��
- ʹ�����ɵ����������ң��Ͳ���`findUsingInfo(subscriberClass)`������apt�н��в��һ�ȡ��

�����`ignoreGeneratedIndex`Ĭ����false�ģ����û������Ŀ������apt��ǿ��ʹ�÷��䷽ʽ���ҡ�

```java
/** Forces the use of reflection even if there's a generated index (default: false). */
public EventBusBuilder ignoreGeneratedIndex(boolean ignoreGeneratedIndex) {
	this.ignoreGeneratedIndex = ignoreGeneratedIndex;
	return this;
}
```

�ڲ���`SubscriberMethod`ʱ��EventBus��װ��һ����`FindState`�Բ��ҵ�״ֵ̬�����ṹ���£�

```java
static class FindState {
	final List<SubscriberMethod> subscriberMethods = new ArrayList<>();
	final Map<Class, Object> anyMethodByEventType = new HashMap<>();
	final Map<String, Class> subscriberClassByMethodKey = new HashMap<>();
	final StringBuilder methodKeyBuilder = new StringBuilder(128);

	Class<?> subscriberClass;
	Class<?> clazz;
	boolean skipSuperClasses;
	SubscriberInfo subscriberInfo;
}
```

�����ж�����`subscriberClass`���¼�����clazz���Լ����ҵĽ��`subscriberMethods`��`subscriberInfo`�ȣ����⣬����һ���жϵı�־��`skipSuperClasses`����������Ƿ���Ҫ���и���Ĳ��ҡ�

���������ֲ��ҷ��������趼��һ���ģ�����`FindState`����Щͨ�õĲ����װ���������������Ĳ���

- initForSubscriber(Class<?> subscriberClass) ��ʼ��������
- checkAdd(Method method, Class<?> eventType) ��鷽���Ϸ���
- checkAddWithMethodSignature(Method method, Class<?> eventType) ��鷽���Ϸ���
- moveToSuperclass() �Ƿ���Ҫ�鿴�����еĶ��ķ���

���˶Բ��ҽ���ķ�װ��FindState��ʹ���˻���أ�

```java
private static final int POOL_SIZE = 4;
private static final FindState[] FIND_STATE_POOL = new FindState[POOL_SIZE];

private FindState prepareFindState() {
	synchronized (FIND_STATE_POOL) {
		for (int i = 0; i < POOL_SIZE; i++) {
			FindState state = FIND_STATE_POOL[i];
			if (state != null) {
				FIND_STATE_POOL[i] = null;
				return state;
			}
		}
	}
	return new FindState();
}
```

EventBusָ����FindState�Ļ���صĴ�СΪ4����ʹ��һά�ľ�̬���飬����������Ҫע���߳�ͬ�������⡣ʹ��ͬ�������ӻ������ȡFindState��ͬ����ͨ���������ַ�ʽ��ȡ`SubscriberMethod`ʱ��Ҳʹ��ͬ������齫FindState�����˻�����С�

```java
private List<SubscriberMethod> getMethodsAndRelease(FindState findState) {
	List<SubscriberMethod> subscriberMethods = new ArrayList<>(findState.subscriberMethods);
	findState.recycle();
	synchronized (FIND_STATE_POOL) {
		for (int i = 0; i < POOL_SIZE; i++) {
			if (FIND_STATE_POOL[i] == null) {
				FIND_STATE_POOL[i] = findState;
				break;
			}
		}
	}
	return subscriberMethods;
}
```

### �ܽ�

������EventBus��Դ��������������Ĺ�����ʽ���������������֮�����ǡ������߷����¼���������ͨ������ķ�ʽ���ݷ����¼���class���Ͳ���SubscriberMethod��Ȼ��ͨ���������invoke�������д����Ӧ�¼��ķ�������������������˿�������Ҫ���Ĺ��������ҽ������ߺͷ�������ȫ���so�Ͻ��������ɡ�

[��ʾ��Demo���ص�ַ](https://github.com/shenhuniurou/BlogDemos/tree/master/EventBusDemo)

## �ο��ĵ�

- [EventBus Documentation](http://greenrobot.org/eventbus/documentation)
- [EventBus 3.0���÷���⣨һ��](https://segmentfault.com/a/1190000004279679)
- [EventBus 3.0���÷����(��)](https://segmentfault.com/a/1190000004314315)
- [ǳ��EventBus 3.0ʵ��˼��](http://alighters.com/blog/2016/05/22/eventbus3-dot-0-analyze/)
- [EventBus��������֪ʶ������](http://www.jianshu.com/p/f8fd67eef9aa)
