.. title: Progress bars with tqdm
.. slug: progress-bars-with-tqdm
.. date: 2017-11-19 07:27:26 UTC+08:00
.. tags: python,programming
.. category: 
.. link: 
.. description: 
.. type: text

At your shell::

  $ pip install tqdm

Whenever you iterate over something, wrap it in a ``tqdm`` like so::

  In [1]: from tqdm import tqdm
  
  In [2]: from time import sleep
  
  In [3]: for i in tqdm(range(1000)):
     ...:     sleep(0.01)
     ...:     
        100%|█████████████████████████████████████████████████████████████████████████████| 1000/1000 [00:10<00:00, 98.49it/s]
        
  In [4]: 



