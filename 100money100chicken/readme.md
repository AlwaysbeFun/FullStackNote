关于百钱百鸡的问题
===
---
在自学php的时候看见了一个非常有意思的问题，大部分人称为百钱百鸡，问题大致是这么说的：
>现在手里有100块，买一只公鸡5块，买一只母鸡3块钱，小鸡1块钱三只，一共需要要买100只鸡，问能买多少只公鸡、母鸡、小鸡。

之后就用php写了一下，下面我就贴出写第一次时的代码：

根据已知条件可以分析出，别的鸡都先不买，就先买公鸡，最多能买100只，同理母鸡也能买100只，小鸡只能买99只（毕竟小鸡1块钱只能买3只），不过没关系也可以按照能买100只来算，后面还有`if`条件来控制。
```
for($gong = 0; $gong <= 100; $gong++){
  for($mu = 0; $mu <= 100; $mu++){
    for($xiao = 0; $xiao < 100; $xiao++){
      if($gong + $mu + $xiao == 100 && $gong * 5 + $mu * 3 + $xiao / 3 == 100){
        echo "公鸡数量:" . $gong . "母鸡数量:" . $mu . "小鸡数量:" . $xiao;
          echo "<br>";
      }
    }
  }
}
```
这是最终的结果：
```
公鸡数量:0母鸡数量:25小鸡数量:75
公鸡数量:4母鸡数量:18小鸡数量:78
公鸡数量:8母鸡数量:11小鸡数量:81
公鸡数量:12母鸡数量:4小鸡数量:84
```
利用三个for循环就可解决问题，之后又去网上看了看，大部分人第一次写首选方式也是这样。由于好奇就添加了一个变量`$n`来记录他的循环次数，结果达到`1020100`次
```
$n = 0; //记录循环结果
for($gong = 0; $gong <= 100; $gong++){
  for($mu = 0; $mu <= 100; $mu++){
    for($xiao = 0; $xiao < 100; $xiao++){
      if($gong + $mu + $xiao == 100 && $gong * 5 + $mu * 3 + $xiao / 3 == 100){
        echo "公鸡数量:" . $gong . "母鸡数量:" . $mu . "小鸡数量:" . $xiao;
          echo "<br>";
      }
      $n++;
    }
  }
}
echo $n; //循环次数结果,1020100
```
之后就一直尝试将循环次数进行降低。最终我将循环次数降到`121`次。

接下来我将贴出来每次优化的代码与结果

**第二次优化的代码：**

再根据条件来分析，一只公鸡5块钱，所以100块只能买20只公鸡；一只母鸡3块钱，所以100块钱只能买33只母鸡；同理100块可以买99只小鸡
```
$n = 0;//记录循环结果
for($gong = 0; $gong <= (100 / 5); $gong++){
  for($mu = 0; $mu <= (100 / 3); $mu++){
    for($xiao = 0; $xiao < 100; $xiao++){
      if($gong + $mu + $xiao == 100 && $gong * 5 + $mu * 3 + $xiao / 3 == 100){
        echo "公鸡数量:" . $gong . "母鸡数量:" . $mu . "小鸡数量:" . $xiao;
        echo "<br>";
      }
      $n++;
    }
  }
}
echo $n;//循环次数结果
```
结果为：
```
公鸡数量:0母鸡数量:25小鸡数量:75
公鸡数量:4母鸡数量:18小鸡数量:78
公鸡数量:8母鸡数量:11小鸡数量:81
公鸡数量:12母鸡数量:4小鸡数量:84
循环次数:71400
```
从第一次和第二次两次对比中发现，已经从循环100万降到7万

**第三次优化的代码**

再次对题目进行分析，鸡的数量一共100只，如果我买了1只公鸡，那么相对应的母鸡的数量就应该是`100 - 1`只，此时小鸡的数量就是`100 - 公鸡 - 小鸡`，下面我们来看代码。
```
$n = 0;
for($gong = 0; $gong <= (100 / 5); $gong++){
  for($mu = 0; $mu <=  100 - $gong; $mu++){
    $xiao = 100 - $gong - $mu;
    if( $gong * 5 + $mu * 3 + $xiao / 3 == 100){
      echo "公鸡数量$gong";
      echo "母鸡数量$mu";
      echo "小鸡数量$xiao";
      echo "<br>";
    } 
    $n++;
  }
}
echo "结果为$n";
```
可以看出本次的代码中没有用到三重循环，就用到两重循环大大降低了循环次数。
结果为：
```
公鸡数量0母鸡数量25小鸡数量75
公鸡数量4母鸡数量18小鸡数量78
公鸡数量8母鸡数量11小鸡数量81
公鸡数量12母鸡数量4小鸡数量84
结果为:1911
```
**第四次代码优化**
我就直接贴代码了，以后有机会了再做更详细的解释
```
$n = 0;
for($gong = 0; $gong <= 100; $gong += 5){
  for($mu = 0; $mu <= 100 - $gong; $mu += 3){
    $xiao = 100 - $gong - $mu;
    if($gong / 5 + $mu / 3 + $xiao * 3 == 100){
      echo "公鸡数量" . ($gong / 5);
      echo "母鸡数量" . ($mu / 3);
      echo "小鸡数量" . $xiao * 3;
      echo "<br>";
    }
    $n++;
  }
}
echo "结果为$n";
```
结果为：
```
公鸡数量0母鸡数量25小鸡数量75
公鸡数量4母鸡数量18小鸡数量78
公鸡数量8母鸡数量11小鸡数量81
公鸡数量12母鸡数量4小鸡数量84
结果为:364
```
**第五次代码优化**
```
$n = 0;
for($gong = 0; $gong <= (100 / 5); $gong++){
  for($mu = 0; $mu <=  (100 - $gong * 5) / 3; $mu++){
    $xiao = 100 - $gong - $mu;
    if($xiao % 3 != 0){continue;}
    if( $gong * 5 + $mu * 3 + $xiao / 3 == 100){
      echo "公鸡数量$gong";
      echo "母鸡数量$mu";
      echo "小鸡数量$xiao";
      echo "<br>";
    } 
    $n++;
  }
}
echo "结果为$n";
```
结果为：
```
公鸡数量0母鸡数量25小鸡数量75
公鸡数量4母鸡数量18小鸡数量78
公鸡数量8母鸡数量11小鸡数量81
公鸡数量12母鸡数量4小鸡数量84
结果为:121
```
- 以上几个优化只是许多方式中的其中几个，在从100万往下优化的时候，还试过出来10万次，1000多次，700多次等等，我在网上查到有用数学方式做出来的，最终结果只经历了4步就出来了。有兴趣的可以去查阅看一看。