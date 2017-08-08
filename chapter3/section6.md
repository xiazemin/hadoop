# php   hadoop实践

$ which php

/usr/local/bin/php

$ vi reducer.php

\#!/usr/local/bin/php

&lt;?php

$ vi mapper.php

$ hadoop jar share/hadoop/tools/lib/hadoop-streaming-2.6.4.jar -input /input/input1.txt -output /output2 -mapper /Users/didi/hadoop/php/mapper.php -reducer /Users/didi/hadoop/php/reducer.php

Caused by: java.lang.RuntimeException: configuration exception

```
at org.apache.hadoop.streaming.PipeMapRed.configure\(PipeMapRed.java:222\)

at org.apache.hadoop.streaming.PipeMapper.configure\(PipeMapper.java:66\)

... 23 more
```

Caused by: java.io.IOException: Cannot run program "/Users/didi/hadoop/php/mapper.php": error=13, Permission denied

$ chmod +x /Users/didi/hadoop/php/mapper.php

$ chmod +x /Users/didi/hadoop/php/reducer.php

$ hadoop jar share/hadoop/tools/lib/hadoop-streaming-2.6.4.jar -input /input/input1.txt -output /output4 -mapper "/Users/didi/hadoop/php/mapper.php" -reducer** "**/Users/didi/hadoop/php/reducer.php"



