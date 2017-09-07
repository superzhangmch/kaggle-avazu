LEARN：从此代码的学习所得
----------------
base 已经效果很好（run & check了下）了，因此先只关心base。
- user_id 的确认：和第三的方法一样。他们怎么想到的？
- 数据拆分：第三的方法中也这样拆分了。他们怎么想到的？
- 去掉不重要特征：看论坛，作者说是去除法多次训练获知哪些feature可去。但是留下的C14，C17，C21等，前三名怎么都差不多都认为他们重要？
- 低频特征变为count特征（也就是按count分组形成0/1特征）：第三方案中也大量这样做。莫非是套路做法吗？
- 特征 hash trick，最后塞给FM模型。奇怪代码中的这个工具的效果，怎么比他们的（好像作者还是他们的）libFM、libFFM 效果好不少呢？
- 其他

---------------
下面为原始 README
---------------

4 Idiots' Approach for Click-through Rate Prediction
====================================================

Our team consists of:
    
    Name              Kaggle ID         Affiliation
    ====================================================================
    Yu-Chin Juan      guestwalk         National Taiwan University (NTU)
    Wei-Sheng Chin    mandora           National Taiwan University (NTU)
    Yong Zhuang       yolicat           National Taiwan University (NTU)
    Michael Jahrer    Michael Jahrer    Opera Solutions

Our final model is an ensemble of NTU's model and Michael's model. Michael's
model is based on his work in Opera Solutions, so he cannot release his part.
Therefore, in the codes and documents we only present NTU's model.

This README introduces how to run our code up. For the introduction to our
approach, please see 

    http://www.csie.ntu.edu.tw/~r01922136/slides/kaggle-avazu.pdf

The model we use for this competition is called `field-aware factorization
machines.' We have released a package for this model at:

    http://www.csie.ntu.edu.tw/~r01922136/libffm



System Requirement
==================

- 64-bit Unix-like operating system

- Python 3

- g++ (with C++11 and OpenMP support)

- pandas (required if you want to run the `bag' part. See `Step-by-step'
  below.)



Step-by-step
============

Our solution is an ensemble of 20 models. It is organized into the following
three parts:
    
    name       public score     private score     description        
    ===========================================================================
    base             0.3832            0.3813     2 basic models

    bag              0.3826            0.3807     2 models using bag features.

    ensemble         0.3817            0.3797     an ensemble of the above 4 
                                                  models and 16 new small models

Because the `bag' part consumes a huge amount of memory (more than 64GB), and
the `ensemble' part takes a long time to run, this instruction guides you to
run our `base' part first. If you want reproduce our best result, please run the
commands in the final step on a suitable machine.


1.  First, please use the following command to run a tiny example up

    $ ./run.sh x

2.  Create a symbolic link to the training dataset

    $ ln -sf <training_set_path> tr.r0.csv

3.  Add a dummy label to the test set

    $ ./add_dummy_label.py <test_set_path> va.r0.csv

4.  Checksum

    $ md5sum tr.r0.csv va.r0.csv
    f5d49ff28f41dc993b9ecb2372abb033  tr.r0.csv
    6edd380a5897bc16b61c5a626062f7b3  va.r0.csv

5.  Reproduce our base submission

    $ ./run.sh 0
    
    Note: base.r0.prd is the submission file

6.  (optional) Reproduce our best submission

    $ ./run_all.sh x

    If success, then run

    $ ./run_all.sh 0

    Note: The algorithm in the `bag' part is non-deterministic. That is, the
    result can be slightly different when you run it two or more times.



==============

If you want to trace these codes, please be prepared that it will take some
efforts. We do not have enough time to polish the codes here to improve the
readability. Sorry about it. 

For any questions and comments, please send your email to:

    Yu-Chin (guestwalk@gmail.com)
