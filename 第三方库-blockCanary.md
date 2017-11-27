
### BlockCanary卡顿检测神器

核心出发点，Looper里面的一个printer
```
// This must be in a local variable, in case a UI event sets the logger
            final Printer logging = me.mLogging;
	    //消息run开始
            if (logging != null) {
                logging.println(">>>>> Dispatching to " + msg.target + " " +
                        msg.callback + ": " + msg.what);
            }

            final long traceTag = me.mTraceTag;
            if (traceTag != 0 && Trace.isTagEnabled(traceTag)) {
                Trace.traceBegin(traceTag, msg.target.getTraceName(msg));
            }
            try {
                msg.target.dispatchMessage(msg);
            } finally {
                if (traceTag != 0) {
                    Trace.traceEnd(traceTag);
                }
            }
	    //消息run结束
            if (logging != null) {
                logging.println("<<<<< Finished to " + msg.target + " " + msg.callback);
            }
```

原理简要阐述一下吧

1.Android的主线程模型，所有的主线程消息都是跑在这个Looper中。

2.两个消息之间的执行间隔，可以计算出消耗时间。

3.超过自定义的卡顿阀值，可以打印出线程call-stack

4.问题找到，去分析吧。
