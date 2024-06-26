##理论

数组就是内存中按照连续的地址排放的一系列数据。其中

- 数组下标从0开始
- 数组内存空间的地址是连续的，高维数组中的地址也是连续排布的。

在C++中，数组的元素是不能删除的，只能覆盖。



##数组的内存

一个 C++数组的大小是在声明后就固定了。

在 C++中,当你声明一个数组时,你**必须指定数组的大小**。这个大小告诉编译器数组中有多少个元素,并且编译器会在内存中为这些元素分配适当的空间。

一旦数组被声明,它的大小就不能被修改。你不能增加或减少数组的元素数量,因为这将涉及到在内存中重新分配空间,并且需要将现有元素复制到新的空间中。

需要注意的是,如果你需要一个可以根据需要自动调整大小的数组,你可以使用动态内存分配,例如**使用`new`操作符和`delete`操作符**来在运行时动态分配和释放数组。但是,这样的数组需要手动管理内存,如果不正确地管理内存,可能会导致内存泄漏或其他问题。



##相关练习题

1、将矩阵按对角线排序：https://leetcode.cn/problems/sort-the-matrix-diagonally/

思路：找到矩阵各个对角线元素横纵坐标之间的数学关系，枚举每一条对角线，暴力地分情况工作量大且低效。

