# BigBang

BigBang is a toolkit for studying communications data from collaborative
projects. It currently supports analyzing mailing lists from Sourceforge,
Mailman, or [.mbox][mbox] files.

[mbox]: http://tools.ietf.org/html/rfc4155

## Installation

BigBang depends on several scientific computing packages that you must first install on your system, which include:

* [numpy](http://docs.scipy.org/doc/numpy/user/install.html)
* [matplotlib](http://matplotlib.org/users/installing.html)
* [graphviz](http://www.graphviz.org/)


You can use the [Anaconda](http://continuum.io/downloads) distribution to
install `numpy` and `matplotlib` on almost any platform. This will also install
the `conda` package management system, which you can use to complete
installation. **Note** that Anaconda does not include Graphviz, so you will
have to install that separately.

If you choose not to use Anaconda, you will have to install each of the
above-mentioned packages for your platform. If you're using OS X [these instructions][osx] may be helpful.

[osx]: http://www.lowindata.com/2013/installing-scientific-python-on-mac-os-x/

Once these dependencies are installed, you can install BigBang
using either `conda` or `pip`.

### conda installation

Run the following commands:

```bash
git clone https://github.com/sbenthall/bigbang.git
conda create -n bigbang python
cd bigbang
bash conda-setup.sh
```

### pip installation

Run the following commands:

```bash
git clone https://github.com/sbenthall/bigbang.git
# optionally create a new virtualenv here
pip install -r requirements.txt
python setup.py develop
```

## Usage

There are serveral IPython notebooks in the `examples/` directory of this
repository. To open them and begin exploring, run the following commands in the
root directory of this repository:

```bash
source activate bigbang
ipython notebook examples/
```

### Collecting from Mailman

BigBang comes with a script for collecting files from public Mailman web
archives. An example of this is the
[scipy-dev](http://mail.scipy.org/pipermail/scipy-dev/) mailing list page. To
collect the archives of the scipy-dev mailing list, run the following command
from the root directory of this repository:

```bash
python bin/collect_mail.py -u http://mail.scipy.org/pipermail/scipy-dev/
```

You can also give this command a file with several urls, one per line. One of these is provided in the `examples/` directory.

```bash
python bin/collect_mail.py -f examples/urls.txt
```

Once the data has been collected, BigBang has functions to support analysis.

## Git Information

A new branch of BigBang is collecting git commit information for projects. We can analyze a project using both its mail and gir information to answer new questions about development.

### Collecting Git Information

As of now, the git collection clones targeted repos into `<./git_data/sample_git_repos>` which can take some time. This works similarly to mail collection. While in the bigbang directory, run

```bash
python bin/collect_git.py -u https://github.com/scipy/scipy.git
```

You can also give this command a file with several urls, one per line. One of these is provided in the `examples/` directory.

```bash
python bin/collect_git.py -f examples/git_urls.txt
```

### Loading Git Information

After the git repositories have been cloned locally, you will be able to start analyzing them. To do this, you will need a GitRepo object, which is a convenient wrapper which does the work of extracting and generating most of the git information and storing it internally in a pandas dataframe. You can then use this GitRepo object's methods to gain access to the large pandas dataframe.

There are three main ways to generate a GitRepo object for a repository, using RepoLoader:

1. By name: `git_repo_obj = RepoLoader.get_repo("bigbang", "name")`. The first term is the name of the folder we cloned that repo into.
2. By absolute address `git_repo_obj = RepoLoader.get_repo("~/path/to/repo", "local")`. This is useful for when you already have the repo locally and don't want to copy it over into the `sample_git_repos` folder.
3. By remote URL `git_repo_obj = RepoLoader.get_repo("https://github.com/sbenthall/bigbang.git", "remote")` This is when you don't want to unneccesarily re-run a git collection script and want to be able to analyze individual repos. This will first clone the repo and then load its data normally.

Afterwards, you can directly access the generated pandas dataframe with `git_repo_obj.commit_data`

## Community

If you are interested in participating in BigBang development, please subscribe to the [BigBang-dev mailing list](https://lists.sudoroom.org/listinfo/bigbang-dev).

If you are using BigBang and would like support from the core development team, please address your questions to the [BigBang-user mailing list](https://lists.sudoroom.org/listinfo/bigbang-user).

## License

GPLv2, see LICENSE for its text.
