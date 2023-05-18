# 算法

快排

# 练习题

## 【2363】

暴力解法

```java
class Solution {
    public List<List<Integer>> mergeSimilarItems(int[][] items1, int[][] items2) {
        int size1 = items1.length;
        int size2 = items2.length;
        List<List<Integer>> list = new ArrayList<>();
        for (int i = 0; i < size1; i++) {
            int value = items1[i][0];
            int weight = items1[i][1];
            List<Integer> a = Arrays.asList(value, weight);
            List<Integer> list1 = new ArrayList<>(a);
            list.add(list1);
        }
        for (int i = 0; i < size1-1; i++) {
            for (int j = size1-2; j>=i; j--) {
                List<Integer> list1 = list.get(j);
                List<Integer> list2 = list.get(j+1);
                if (list1.get(0)>list2.get(0)){
                    list.set(j+1,list1);
                    list.set(j,list2);
                }
            }
        }
        for (int i = 0; i < size2; i++) {
            int value = items2[i][0];
            int weight = items2[i][1];
            int size = list.size();
            int a = 0,b=size-1;
            while (a<b){
                int temp = (b+a)/2;
                List<Integer> list1 = list.get(temp);
                Integer integer = list1.get(0);
                if (integer>value){
                    b=temp-1;
                }else if (integer==value){
                    list1.set(1,list1.get(1)+weight);
                    list.set(temp,list1);
                    break;
                }else {
                    a=temp+1;
                }
            }
            if (a>=b){
                if (list.get(a).get(0)>value){
                    list.add(a,Arrays.asList(value,weight));
                }else if (list.get(a).get(0)<value){
                    list.add(a+1,Arrays.asList(value,weight));
                }else {
                    List<Integer> list1 = list.get(a);
                    list1.set(1,list1.get(1)+weight);
                    list.set(a,list1);
                }
            }
        }
        return list;
    }
}
```

普通解法





## 【1487】

```java
class Solution {
    public String[] getFolderNames(String[] names) {
        int size = names.length;
        String[] ant = new String[size];
        String temp = "";
        int k = 0 ;
        for (int i = 0; i < size; i++) {
            temp = names[i];
            String tem = "";
            k=0;
            for (int j = 0; j < i; j++) {
                tem = "";
                if (temp.equals(ant[j])){
//                    System.out.println(temp+temp.charAt(temp.length()-1));
                    if (temp.charAt(temp.length()-1)==')'){
                        if (tem.equals("")){
                            int l;
                            for (l = 2;l<=size; l++) {
                                char c = temp.charAt(temp.length()-1*l);
//                                System.out.println(c);
                                if (c=='(') break;
//                        k = (int) (k+(c*Math.pow(10,l-1)));
                            }
                            tem = temp.substring(0,temp.length()-l);
                        }
                    }else {
                        tem = temp;
                    }
                    System.out.println("tem=="+tem+",temp = "+temp);
                    temp = tem+"("+(++k)+")";
                    j=-1;
                }
            }
            ant[i] = temp;
        }
        return ant;
    }
}
```

暴力解法

```java
class Solution {
    public String[] getFolderNames(String[] names) {
        int size = names.length;
        String[] ant = new String[size];
        Map<String,Integer> index = new HashMap<>();
        String temp = "";
        for (int i = 0; i < size; i++) {
            String name = names[i];
            if (!index.containsKey(name)){
                ant[i] = name;
                index.put(name,1);
            }else {
                int k = index.get(name);
                while (index.containsKey(addSuffix(name,k))){
                    k++;
                }
                ant[i]=addSuffix(name,k);
                index.put(name,k+1);
                index.put(addSuffix(name,k),1);
            }
        }
        return ant;
    }
    public String addSuffix(String name, int k) {
        return name + "(" + k + ")";
    }
}
```



## 【231】

```java
class Solution {
    public String[] findLongestSubarray(String[] array) {
        int prenum = 0;
        int preword = 0;
        int afternum = 0;
        int afterword = 0;
        int num = 0;
        int maxnum =0;
        int maxb = 0;
        int length = array.length;
        for (int i = 0; i < length; i++) {
            if (array[i].charAt(0)<='z'){
                if (afterword==0){
                    num = Math.min(preword,afternum);
                    if (num>maxnum){
                        maxb = i-1;
                        maxnum = num;
                    }
                    afterword = 1;
                    prenum = afternum;
                    afternum = 0;
                }else {
                    afterword++;
                }

            }
            else {
                if (afternum==0){
                    num = Math.min(afternum,prenum);
                    afternum = 1;
                    preword  = afterword;
                    afterword = 0;
                    if (num>maxnum){
                        maxb = i-1;
                        maxnum = num;
                    }
                }else {
                    afternum++;
                }
            }
        }
        if (afternum!=0){
            num = Math.min(preword,afternum);
            if (num>maxnum){
                maxb = length-1;
                maxnum = num;
            }
        }else {
            num = Math.min(afternum,prenum);
            if (num>maxnum){
                maxb = length-1;
                maxnum = num;
            }
        }
        System.out.println(maxnum);

        return Arrays.copyOfRange(array,maxb-(maxnum*2)+1,maxb);
    }
}
```



## 【1012】

```java
class Solution {
    public int numDupDigitsAtMostN(int n) {
        if(n<11) return 0;
        int temp = n;
        int length = 0;
        int pracity = 0;//当前的总容量9999
        // 100
        int pracity0 = 0; // 上一个容量0101
        // 1001

        int pracity01 = 0;
        while (temp>0){
            length++;
            temp/=10;
        }
//        System.out.println(length);
        int[] pracitys = new int[length];
        int sum = 0 ;
        pracitys[0] = 0;
        // 0-99
        // 0--999
        // 0--9999
        for (int i = 1; i < length; i++) {
//            pracity = (pracity+i)*9;//没有考虑到1001现象
//            temp = pracity;
            // eg. 9999 = 0-999的值 + 头位在999每个位置出现一次 +99的值（此位置0的情况）
//            pracity = (pracity+i+pracity0)*9;
            if (i==1){
                pracity = (pracitys[i-1]+i);
            }else {
                int num = 9;
                temp = 1;
                for (int j = 1; j < i; j++) {
                    temp *=num;
                    num--;
                }
                temp = temp*i;
                // 4 10000
                // 9999
                // 9^3
                // 999
                pracity = (pracitys[i-1])*10+temp;
            }
            pracitys[i] = pracity;
            System.out.println(temp +" "+pracitys[i]);
//            sum+=pracity;
        }
//        sum += n/(10^length)*pracitys[length-1];
        for (int i = length-1; i > 0; i--) {
            int temp1 = n/(int)Math.pow(10,i)%10;
            System.out.println(temp1+" "+Math.pow(10,i));
            if (temp1>1)
                sum += (temp1-1)*pracitys[i];
            else if (temp1==1){
                if (i>0){
                    sum += 9*pracitys[i-1];
                }
            }else {
//                sum+=pracitys[i];
            }

        }
        return sum;
    }
}
```

