Two Sum 解法笔记
题目
给一个整数列表 nums 和一个目标值 target，找出两个数，使它们相加等于 target，返回这两个数的索引。
假设每个输入只有一个答案，同一个元素不能重复使用。

示例：

python
nums = [2, 7, 11, 15]
target = 9
# 返回 [0, 1]，因为 nums[0] + nums[1] = 2 + 7 = 9
核心思路：哈希表，一次遍历
一边遍历，一边把“见过的数字 → 它的索引”存进字典。
每看到一个 num，就算出 difference = target - num，然后查字典里有没有 difference。
如果有，直接返回两个索引；如果没有，把当前 num 和索引存进去。

正确代码
python
class Solution:
    def twoSum(self, nums: list[int], target: int) -> list[int]:
        seen = {}
        for i, num in enumerate(nums):
            difference = target - num
            if difference in seen:
                return [seen[difference], i]
            seen[num] = i
逐行解释
代码	含义
seen = {}	创建一个空字典，用来存 {数字: 索引}
for i, num in enumerate(nums):	i 是索引，num 是当前数字
difference = target - num	我还需要哪个数才能凑成 target
if difference in seen:	之前见过这个数吗？
return [seen[difference], i]	见过，返回之前那个数的索引和当前索引
seen[num] = i	没见过，把当前数字和索引存进字典
容易错 / 搞混的地方
❌ 1. seen 是字典，不是列表
python
seen = {}      # 空字典
seen = []      # 空列表（错）
字典才能用 seen[num] = i 存键值对。

列表只能按位置存，不能这样用。

❌ 2. 键值对含义搞反
python
seen[num] = i   # 正确：键=数字值，值=索引
seen[i] = num   # 错误：键=索引，值=数字
因为我们要用 difference（数字值）去查，所以键必须是数字值。

最终返回的是索引，所以值存索引。

❌ 3. 忘记存 seen[num] = i
python
if difference in seen:
    return ...
# 少了这一行：seen[num] = i
不存的话，seen 永远是空的，永远查不到。

❌ 4. 先存后查，顺序反了
python
seen[num] = i        # 先存
if difference in seen:  # 后查
如果 target = 2 * num，可能会找到自己。

正确顺序：先查，再存。

❌ 5. 返回的是值，不是索引
python
return [num, seen[difference]]   # 错：num 是值
return [seen[difference], i]     # 对：两个都是索引
题目要的是索引，不是数字本身。

❌ 6. 变量名写错
python
seen[n] = i   # 错：n 没定义
seen[num] = i # 对
循环变量是 num，不要写成 n。

❌ 7. enumerate 不熟
python
for i, num in enumerate(nums):
等价于：

python
for i in range(len(nums)):
    num = nums[i]
i 是索引，num 是值。

返回时用 i，不要用 num。

❌ 8. 暴力法两层循环写成自己加自己
python
for i in range(len(nums)):
    for j in range(len(nums)):   # 错：j 从 0 开始
应该：

python
for j in range(i + 1, len(nums)):  # 对：只找后面的数
手动走一遍例子
nums = [3, 2, 4], target = 6

步骤	i	num	difference	seen 里有什么？	动作
1	0	3	3	{}	3 不在，存 seen[3] = 0
2	1	2	4	{3: 0}	4 不在，存 seen[2] = 1
3	2	4	2	{3: 0, 2: 1}	2 在！返回 [seen[2], 2] = [1, 2]
答案 [1, 2]，因为 nums[1] + nums[2] = 2 + 4 = 6。

复杂度
方法	时间	空间
暴力法（两层循环）	O(n²)	O(1)
哈希表（本解法）	O(n)	O(n)
时间：只遍历一次。

空间：最坏情况下存所有数字。

自测清单
写完代码后问自己：

□ seen 是 {} 吗？
□ seen[num] = i 写了吗？
□ 是先查 difference in seen，再存 seen[num] = i 吗？
□ 返回的是 [seen[difference], i] 吗？
□ num 和 n 没写混吧？
□ enumerate 的 i 和 num 分清楚了吗？
记忆口诀
字典存数对，键是值来值是位。
先查差在否，再存当前数和位。
返回两个位，不是两个数。

