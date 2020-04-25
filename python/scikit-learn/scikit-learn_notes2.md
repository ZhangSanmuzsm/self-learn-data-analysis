# scikit-learn笔记2——Setting up Python for machine learning: scikit-learn and Jupyter Notebook
## Agenda
- What are the benefits and drawbacks of scikit-learn?
- How do I install scikit-learn?
- How do I use the Jupyter Notebook?
- What are some good resources for learning Python?

## Benefits and drawbacks of scikit-learn
### Benefits:
- **Consistent interface** to machine learning models
- Provides many **tuning parameters** but with **sensible defaults**
- Exceptional **documentation**
- Rich set of functionality for **companion tasks**
- **Active community** for development and support
### Potential drawbacks:
- Harder (than R) to **get started with machine learning**
- Less emphasis (than R) on **model interpretability**

### Further reading:
- Ben Lorica: [Six reasons why I recommend scikit-learn](http://radar.oreilly.com/2013/12/six-reasons-why-i-recommend-scikit-learn.html)
- scikit-learn authors: [API design for machine learning software](http://arxiv.org/pdf/1309.0238v1.pdf)
- Data School: [Should you teach Python or R for data science?](http://www.dataschool.io/python-or-r-for-data-science/)

## Installing scikit-learn
**Option 1:** [Install scikit-learn library](http://scikit-learn.org/stable/install.html) and dependencies (NumPy and SciPy)
**Option 2:** [Install Anaconda distribution](https://www.anaconda.com/download/) of Python, which includes:
- Hundreds of useful packages (including scikit-learn)
- IPython and Jupyter Notebook
- conda package manager
- Spyder IDE

## Using the Jupyter Notebook

### Components:
- **IPython interpreter:** enhanced version of the standard Python interpreter
- **Browser-based notebook interface:** weave together code, formatted text, and plots

### Installation:

- **Option 1:** [Install the Jupyter notebook](https://jupyter.readthedocs.io/en/latest/install.html) (includes IPython)
- **Option 2:** Included with the Anaconda distribution

### Launching the Notebook:

- Type **jupyter notebook** at the command line to open the dashboard
- Don't close the command line window while the Notebook is running

### Keyboard shortcuts:
**Command mode** (gray border)
- Create new cells above (**a**) or below (**b**) the current cell
- Navigate using the **up arrow** and **down arrow**
- Convert the cell type to Markdown (**m**) or code (**y**)
- See keyboard shortcuts using **h**
- Switch to Edit mode using **Enter**

**Edit mode** (green border)

- **Ctrl+Enter** to run a cell
- Switch to Command mode using **Esc**

**补充**
现在我们已经开始了，最好的做法是定期存储。按 Ctrl + S 键可以通过调用“保存和检查点”命令来保存你的 notebook，但是这个检查点又是什么呢?

每当你创建一个新的 notebook 时，都会创建一个检查点文件以及你的 notebook 文件；它将位于你保存位置的隐藏子目录中称作 .ipynb_checkpoints，也是一个 .ipynb 文件。默认情况下，Jupyter 将每隔 120 秒自动保存你的 notebook，而不会改变你的主 notebook 文件。当你“保存和检查点”时，notebook 和检查点文件都将被更新。因此，检查点使你能够在发生意外事件时恢复未保存的工作。你可以通过 “File > Revert to Checkpoint“ 从菜单恢复到检查点。

### IPython, Jupyter, and Markdown resources:

- [nbviewer](http://nbviewer.jupyter.org/): view notebooks online as static documents
- [IPython documentation](http://ipython.readthedocs.io/en/stable/)
- [Jupyter Notebook quickstart](http://jupyter.readthedocs.io/en/latest/content-quickstart.html)
- [GitHub's Mastering Markdown](https://guides.github.com/features/mastering-markdown/): short guide with lots of examples

## Resources for learning Python

- [Codecademy's Python course](https://www.codecademy.com/learn/learn-python): browser-based, tons of exercises
- [DataQuest](https://www.dataquest.io/): browser-based, teaches Python in the context of data science
- [Google's Python class](https://developers.google.com/edu/python/): slightly more advanced, includes videos and downloadable exercises (with solutions)
- [Python for Everybody](https://www.py4e.com/): beginner-oriented book, includes slides and videos

