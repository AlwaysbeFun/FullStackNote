- 快速排列算法原理
例如一个数组`$arr = [5,10,3,1,7];`，选出第一项`5`作为中间项，然后依次和数组中的其他元素进行比较，比中间项大的放在右边组成一个新的右边数组，比中间项小的放在左边组成一个新的左边数组。
  - 如果此时左边数组和右边数组都是已经排好序的，那么将中间项作为中间数组，直接将这三个数组直接相连就可以。
  - 如果左右两边数组没有排好序，那么此时就要利用到**递归**进行排序
- 代码示例：
  ```
    $arr = [5,10,3,1,7];
    function getArr($arr){
      $arrlen = count($arr);
      if($arrlen <= 1){  
        //这里是跳出递归循环的关键，如果长度为1或者0那么就意味着排序已经进行完成了，仔细想一想如果数组长度为1了，
        //那就是说只有一项了，无法再进行左右两边比较排序，此时也就是为排序完成的时候
        return $arr;
      }
      $arrmid = $arr[0]; //获取第一项
      $arrmid_array = array($arrmid); //将第一项也作为一个数组
      $arrleft = array(); //左边数组
      $arrright = array(); //右边数组
      for($i = 1; $i < $arrlen; $i++){ //从第二项开始循环数组
        if($arr[$i] < $arrmid){  //比中间项小放左边
          $arrleft[] = $arr[$i];
        }
        else{
          $arrright[] = $arr[$i]; //比中间项大的放右边
        }
      }
      $arrleft = getArr($arrleft); //如果左边没有拍好序，则利用递归继续进行循环
      $arrright = getArr($arrright); //如果右边没有拍好序，则利用递归继续进行循环
      $resultarr = array_merge($arrleft, $arrmid_array,$arrright); //将最终得到的左中右连接为一个数组
      return $resultarr;
    }
    $rlt = getArr($arr);
    print_r($rlt);
  ```