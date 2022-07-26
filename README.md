# exdir-cpp



## Getting started

To make it easy for you to get started with GitLab, here's a list of recommended next steps.

Already a pro? Just edit this README.md and make it your own. Want to make it easy? [Use the template at the bottom](#editing-this-readme)!

## Add your files

- [ ] [Create](https://docs.gitlab.com/ee/user/project/repository/web_editor.html#create-a-file) or [upload](https://docs.gitlab.com/ee/user/project/repository/web_editor.html#upload-a-file) files
- [ ] [Add files using the command line](https://docs.gitlab.com/ee/gitlab-basics/add-file.html#add-a-file-using-the-command-line) or push an existing Git repository with the following command:

```
cd existing_repo
git remote add origin http://git.bonn-hungary.local:8888/bhespace/exdir-cpp.git
git branch -M main
git push -uf origin main
```

## Integrate with your tools

- [ ] [Set up project integrations](http://git.bonn-hungary.local:8888/bhespace/exdir-cpp/-/settings/integrations)

## Collaborate with your team

- [ ] [Invite team members and collaborators](https://docs.gitlab.com/ee/user/project/members/)
- [ ] [Create a new merge request](https://docs.gitlab.com/ee/user/project/merge_requests/creating_merge_requests.html)
- [ ] [Automatically close issues from merge requests](https://docs.gitlab.com/ee/user/project/issues/managing_issues.html#closing-issues-automatically)
- [ ] [Enable merge request approvals](https://docs.gitlab.com/ee/user/project/merge_requests/approvals/)
- [ ] [Automatically merge when pipeline succeeds](https://docs.gitlab.com/ee/user/project/merge_requests/merge_when_pipeline_succeeds.html)

## Test and Deploy

Use the built-in continuous integration in GitLab.

- [ ] [Get started with GitLab CI/CD](https://docs.gitlab.com/ee/ci/quick_start/index.html)
- [ ] [Analyze your code for known vulnerabilities with Static Application Security Testing(SAST)](https://docs.gitlab.com/ee/user/application_security/sast/)
- [ ] [Deploy to Kubernetes, Amazon EC2, or Amazon ECS using Auto Deploy](https://docs.gitlab.com/ee/topics/autodevops/requirements.html)
- [ ] [Use pull-based deployments for improved Kubernetes management](https://docs.gitlab.com/ee/user/clusters/agent/)
- [ ] [Set up protected environments](https://docs.gitlab.com/ee/ci/environments/protected_environments.html)

***

# Editing this README

When you're ready to make this README your own, just edit this file and use the handy template below (or feel free to structure it however you want - this is just a starting point!). Thank you to [makeareadme.com](https://www.makeareadme.com/) for this template.

## Suggestions for a good README
Every project is different, so consider which of these sections apply to yours. The sections used in the template are suggestions for most open source projects. Also keep in mind that while a README can be too long and detailed, too long is better than too short. If you think your README is too long, consider utilizing another form of documentation rather than cutting out information.

## Name
Choose a self-explaining name for your project.

## Description
Let people know what your project can do specifically. Provide context and add a link to any reference visitors might be unfamiliar with. A list of Features or a Background subsection can also be added here. If there are alternatives to your project, this is a good place to list differentiating factors.

## Badges
On some READMEs, you may see small images that convey metadata, such as whether or not all the tests are passing for the project. You can use Shields to add some to your README. Many services also have instructions for adding a badge.

## Visuals
Depending on what you are making, it can be a good idea to include screenshots or even a video (you'll frequently see GIFs rather than actual videos). Tools like ttygif can help, but check out Asciinema for a more sophisticated method.

## Installation
Within a particular ecosystem, there may be a common way of installing things, such as using Yarn, NuGet, or Homebrew. However, consider the possibility that whoever is reading your README is a novice and would like more guidance. Listing specific steps helps remove ambiguity and gets people to using your project as quickly as possible. If it only runs in a specific context like a particular programming language version or operating system or has dependencies that have to be installed manually, also add a Requirements subsection.

## Usage
Use examples liberally, and show the expected output if you can. It's helpful to have inline the smallest example of usage that you can demonstrate, while providing links to more sophisticated examples if they are too long to reasonably include in the README.

## Support
Tell people where they can go to for help. It can be any combination of an issue tracker, a chat room, an email address, etc.

## Roadmap
If you have ideas for releases in the future, it is a good idea to list them in the README.

## Contributing
State if you are open to contributions and what your requirements are for accepting them.

For people who want to make changes to your project, it's helpful to have some documentation on how to get started. Perhaps there is a script that they should run or some environment variables that they need to set. Make these steps explicit. These instructions could also be useful to your future self.

You can also document commands to lint the code or run tests. These steps help to ensure high code quality and reduce the likelihood that the changes inadvertently break something. Having instructions for running tests is especially helpful if it requires external setup, such as starting a Selenium server for testing in a browser.

## Authors and acknowledgment
Show your appreciation to those who have contributed to the project.

## License
For open source projects, say how it is licensed.

## Project status
If you have run out of energy or time for your project, put a note at the top of the README saying that development has slowed down or stopped completely. Someone may choose to fork your project or volunteer to step in as a maintainer or owner, allowing your project to keep going. You can also make an explicit request for maintainers.
=======
# Exdir for C++
![License](https://img.shields.io/github/license/HunterBelanger/exdir-cpp.svg)

This library is a C++ implementation of the Exdir specification for storing data.
Exdir is methodology of organizing and storing data, which is meant to replace 
HDF5. As with HDF5, data is stored in a heirarchy, but instead of using a single 
binary file, Exdir uses the filesystem of the users computer to establish this 
heirarchy. Within an Exdir file (which is just a folder on your computer), there
can be Groups, Datasets, or Raws. Groups are used to categorize information, and
groups may be nested within one antoher, and one group may have multiple other
groups as well. Datasets exist in groups, and contain numeric data, stored in a
Numpy npy binary file. Datasets may not contain anyting else except a Raw. Raws
are used to store raw data in other formats. These may be found inside of a
Group, or a Dataset. 

To learn more about Exdir the paper describing the specification can be found 
[here](https://www.frontiersin.org/articles/10.3389/fninf.2018.00016/full), and 
the reference implentation (written for Python) can be found 
[here](https://github.com/CINPLA/exdir).

## Usage
Here is a brief example of exdir-cpp in action
```cpp
#include<exdir/exdir.hpp>

int main() {

  exdir::File file = exdir::create_file("test.exdir");

  exdir::Group group1 = file.create_group("group1");

  std::vector<int> test_data { 1,  2,  3,  4,
                               5,  6,  7,  8,
                               9, 10, 11, 12,
                              13, 14, 15, 16};

  exdir::NDArray<int> test_array(test_data, {4,4});

  test_array.reshape({4,2,2});

  exdir::Dataset<int> dset_1 = group1.create_dataset<int>("data_set_1", test_array);

  dset_1.attrs["density"] = 12.4;

  dset_1.data(0,0,0) = 17.;

  dset_1.data.reshape({4,4});

  return 0;
}
```
A more in-depth explination of the classes and usage is given in the wiki 
[here](https://github.com/HunterBelanger/exdir-cpp/wiki/Usage).

## Dependencies
This package is dependent on the [yaml-cpp](https://github.com/jbeder/yaml-cpp)
library, to handle all interactions with the ```.yaml``` files. It is present in
most distros repositories, and can usually be installed with your system's
package manager.

* **Ubuntu / Linux Mint :**
```sudo apt install libyaml-cpp-dev```

* **Fedora :**
```sudo dnf install yaml-cpp-devel```

* **Arch / Manjaro :**
```sudo pacman -S yaml-cpp```

## Install
All that is required to build exdir-cpp is a C++ compiler capable of the C++17
standard. This library does indeed use C++17 features, and will not build 
without them. This should not be an issue for most useers however, as any 
somewhat modern version of GCC or Clang should be more than adaquet. The 
library should build just fine on Unix  like systems. There is nothing 
preventing this project from being build on Windows systems either, accept for
the fact that those options have yet to be added or tested to the cmake files.

To build on Unix systems, navigate to the directory where you wish to store the 
files and then run the following:
```
git clone https://github.com/HunterBelanger/exdir-cpp.git
cd exdir-cpp
mkdir build && cd build
cmake ..
make -j
```
If you would like to install the library and header file to your system's
default location, you may then run ```sudo make install``` after. You can
also set the install location yourself by running cmake with the
```
