### **TOP-K问题**

需求描述：在一大堆数(数量n)中求其前k大或前k小的问题，简称TOP-K问题。

方案一：最方便的方法就是排序，然后取前k个数。时间复杂度：O(nlogn)

方法二：方案一是对所有元素进行排序，其实对于求前K值，不需要排序整体，只需构建前K个数值即可。那可以构建容量为K最大/最小堆。对于每一个元素，维护堆的代价为O(K)，总消耗变更为O(nlogk);

方法三：另一种思路，借助快排的思想。

快排的每次划分，小于等于分界值 `pivot` 的元素的都会被放到数组的左边，大于的都会被放到数组的右边，然后返回分界值的下标。与快速排序不同的是，快速排序会根据分界值的下标递归处理划分的两侧，而这里我们只处理划分的一边。对于每次pivot，pos是分界线的序号序号，它将此区间分解成了left和right，那么有以下几种情况：

- k == pos+left，此刻，pos就是分界线
- k < pos + left，那么说明结果k在pos左侧，在left区间，继续递归求解left区间。
- k > pos + left，那么说明结果k在pos右侧，在right区间，继续递归求解right区间。

**那怎么求证复杂度？**

假设每次的区间分割都能使区间一分为二，那么最终理想的分割需要logn次，记为m。设整体期望复杂度为F(n)

对于第i次分割，所需要的消耗的次数趋于n*(1^2)^i（i∈[1,m]），f(n) = n * (1/2) + n * (1/4) + ··· + n * (1/2)^m,

抽出n，f(n)=n*g(m)，g(m)为底数为1/2的等比数列的前m项和，所以f(n) <= n，所以可以计算出期望复杂度为 O(n)。

但之前的假设是，每次能将区间分割为2份相同长度的区间，那么最快的情况呢？情况最差时，每次的划分点都是最大值或最小值，一共需要划分 n - 1*n*−1 次，而一次划分需要线性的时间复杂度，所以最坏情况下时间复杂度为O(n^2)。

方法四：方法三的最坏或较坏情况，源于选取分界值时的不确定因素。那如何通过一个好的方式去选取一个分界值去做区间划分呢？

能将区间平分为二份的，那么中位数作为分界值将是一个好选择。（或接近中位数的值）

如果将每个元素当做一组，即有n组，很显然，这种情况求中位数（假设需要消耗O(n)）,很显然能定位到n/2的元素比中位数小。（第i次定位需要消耗n*(1^2)^i）

下面换个思路：方法一是将一个元素作为一组，那么扩展这个组的个数呢？

设p为组的元素个数，那么操作将会变成如下步骤：

- a：选取中位数节点
  - ① 将n个元素划分为n/p组，每组p个元素
  - ② 定位到p个元素组的中位数节点
  - ③ 步骤②中求出的中位数节点，继续用此方法求出中位数（n/p个中位数节点中的中位数）。
- b：步骤③中选取的中位数，按照此为标准，大于中位数的中位数节点放右边，小于则放左边（快排思路）
- c：用步骤③选取的中位数位置与k进行判断，计算k的相对位置，或递归操作。

时间复杂度如何分析？

我们分步骤计算，令组元素个数等于P(为了简化，方便找每组的中位数，P取奇数)，令T(N)为算法的时间复杂度，那么：

①对于步骤寻找所有组的中位数复杂度为T(N/P)。(总共N/P个节点)

②对于每一个组，组内的中位数能确定(P+1)/2个数是大于等于还是小于等于本身，所以对于已求出的组数(假定M组)，可以确定其中的M*(P+1)/2个数满足条件。所以此步骤的消耗情况约为T(N-N * ((P+!)/2) * (1/2P)) = T(((3P-1)/4P)N)

③ 此外，排序每组组内元素，也需要消耗额外的性能，所以这些额外操作令其等于 c*N

结合①②③，可以得出T(N) ≤T(N/P)+T(((3P-1)/4P)N)+ c*N;

令T(N) = f*N，代入上式，f * N  ≤f * N * (3P+3)/4P + c * N

f ≤ f * (3P+3)/4P + c，归边处理得(P-3)/4P * f ≤  c;

因为c是常数，那么f也为常数。所以T(N) = O(N) 

**延伸**

在常见的BFPRT算法中，通常选取5作为P。

这跟T(N)表达式有关，这主要取决于 (3P+3)/4P和c的平衡。选取3，增加 (3P+3)/4P，但是对于组内插入排序的效率更高，选取7,9,11，降低了(3P+3)/4P但却增加了c的常数。综合最少值选取了5。（偶数直接忽略，选取中位数不方便）

JS版代码示例：

```js
//插入排序
function insertSort(arr, l, r) {
    for(let i = l + 1; i <= r; i++)
    {
        if(arr[i - 1] > arr[i])
        {
            let t = arr[i];
            let j = i;
            while(j > l && arr[j - 1] > t)
            {
                arr[j] = arr[j - 1];
                j--;
            }
            arr[j] = t;
        }
    }
}
 
//寻找中位数的中位数
function findMid(arr, l, r) {
    if(l == r) return l;
    let i = 0;
    let n = 0;
    for(i = l; i < r - 5; i += 5)
    {
        insertSort(arr, i, i + 4);
        n = i - l;
        let t = arr[i + 2];
        arr[i + 2] = arr[Math.floor(l + n / 5)];
        arr[Math.floor(l + n / 5)] = t;
    }
 
    //处理剩余元素
    let num = r - i + 1;
    if(num > 0)
    {
        insertSort(arr, i, i + num - 1);
        n = i - l;
        let t = arr[Math.floor(l + n / 5)];
        arr[Math.floor(l + n / 5)] = arr[Math.floor(i + num / 2)];
        arr[Math.floor(i + num / 2)] = t;
    }
    n = Math.floor(n / 5);
    if(n == l) return l;
    return findMid(arr, l, l + n);
}
 
//进行划分过程
function partion(arr, l, r, p)
{
    let t = arr[l];
    arr[l] = arr[p];
    arr[p] = t;
    let i = l;
    let j = r;
    let pivot = arr[l];
    while(i < j)
    {
        while(arr[j] >= pivot && i < j) {
            j--;
        }
        arr[i] = arr[j];
        while(arr[i] <= pivot && i < j) {
            i++;
        }
        arr[j] = arr[i];
    }
    arr[i] = pivot;
    return i;
}

/**
 * 
 * @param {*} arr 
 * @param {*} l 
 * @param {*} r 
 * @param {*} k 
 * @returns 
 */
function BFPRT(arr, l, r, k) {
    let p = findMid(arr, l, r);    //寻找中位数的中位数
    let i = partion(arr, l, r, p);
    let m = i - l + 1;
    if(m == k) {
        return arr[i];
    } else if(m > k) {
        return BFPRT(arr, l, i - 1, k);
    }
    return BFPRT(arr, i + 1, r, k - m);
}
// let data = [1,55,3,1,4,85,1,2,4,6];
// BFPRT(data, 0, data.length-1, Math.floor(data.length/2));
```

