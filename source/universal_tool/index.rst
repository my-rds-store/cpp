通用工具
==============

Pair 和 TUple
`````````````````

Pair
`````````

TUple
``````````````

Smart Pointer
---------------



Class shared_ptr
`````````````````
* `std::shared_ptr <https://zh.cppreference.com/w/cpp/memory/shared_ptr>`_

托管 Pointer 的 最后一个shared_ptr 象在reference被删除是时,会自动执行delete释放指针。 


.. code-block:: cpp

    #include <iostream>
    #include <memory>

    using namespace std;
    class A
    {
    public:
        int i;
        A(int n):i(n) { };
        ~A() { cout << i << " " << "destructed" << endl; }
    };
    int main()
    {
        shared_ptr<A> sp1(new A(2)); /* A(2)由sp1托管， */
        shared_ptr<A> sp2(sp1);      /* A(2)同时交由sp2托管 */
        shared_ptr<A> sp3;


        sp3 = sp2;                   /* A(2)同时交由sp3托管 */
        cout << "use_count : " << sp3.use_count() << endl;
        cout << sp1->i << "," << sp2->i <<"," << sp3->i << endl;

        A * p = sp3.get();          /* get返回托管的指针，p 指向 A(2) */
        cout << p->i << endl;       /* 输出 2 */

        sp1.reset(new A(3));        /* reset导致托管新的指针, 此时sp1托管A(3) */
        cout << "sp1.reset(A(3)), sp3.use_count : " << sp3.use_count() << endl;

        sp2.reset(new A(4));       /* sp2托管A(4) */
        cout << sp1->i << endl;    /* 输出 3 */
        cout << "sp2.reset(A(4)), sp3.use_count : " << sp3.use_count() << endl;

        sp3.reset(new A(5));      /* sp3托管A(5),A(2)无人托管，被delete*/
        cout << "sp3.reset(A(5)), sp3.use_count : " << sp3.use_count() << endl;


        sp3.reset();              /* 复位,A(5)无人托管，被delete*/
        cout << "sp3.reset(); sp3.use_count : " << sp3.use_count() << endl;

        cout << "end" << endl;
        return 0;
    }


.. code::  

    use_count : 3
    2,2,2
    2
    sp1.reset(A(3)), sp3.use_count : 2
    3
    sp2.reset(A(4)), sp3.use_count : 1
    2 destructed
    sp3.reset(A(5)), sp3.use_count : 1
    5 destructed
    sp3.reset(); sp3.use_count : 0
    end
    4 destructed
    3 destructed


Class unique_ptr
`````````````````
* `std::unique_ptr <https://zh.cppreference.com/w/cpp/memory/unique_ptr>`_


