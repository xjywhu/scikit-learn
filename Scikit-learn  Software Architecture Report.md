# Scikit-learn  Software Architecture Report

## Index

- Abstract
- Introduction
- Stakeholders
  - Core team members & contributors
  - Other contributors
  - Users
  - Competitors
- Context View 
- Development View
  - Module Structure Model
  - Source code structure
- Deployment View
  - Dependencies
  - Environment/Runtime Environment
  - Specialist Knowledge
- Functional View 
- Performance and Scalability Perspective 
- Technical Debt
  - Ambiguous Project Structure
  - Messy Module Dependencies
  - Code Inconsistencies
  - Low Code Coverage
  - ......
- Evolution Perspective 
- Conclusion
- References



## Abstract

Scikit-learn was started in 2007 as a Google Summer of Code project by David Cournapeau. It is a tool for machine learning in the Python language. Due to its simplicity and efficiency, it has been one of the most used libraries in machine learning. As an open-source project, there are many developers contributing and keeping it growing. So, we decide to do a stakeholder analysis to figure out the aspect of its development members. Then, we describe scikit-learn from various viewpoints to show off its software architecture. In order to understand the project comprehensively, we do analysis of technical and testing debt in the end of chapter.

### Demo

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import sklearn
from sklearn.datasets import load_boston
boston = load_boston()

from sklearn.linear_model import LinearRegression

bos = pd.DataFrame(boston.data)
bos.columns = boston.feature_names
bos['PRICE'] = boston.target
bos.head()

X = bos.drop('PRICE', axis=1)
lm = LinearRegression()
lm.fit(X, bos.PRICE)

print('线性回归算法w值：', lm.coef_)
print('线性回归算法b值: ', lm.intercept_)

import matplotlib.font_manager as fm
myfont = fm.FontProperties(fname='C:/Windows/Fonts/msyh.ttc')
plt.scatter(bos.RM, bos.PRICE)
plt.xlabel(u'住宅平均房间数', fontproperties=myfont)
plt.ylabel(u'房屋价格', fontproperties=myfont)
plt.title(u'RM与PRICE的关系', fontproperties=myfont)
plt.show()
```

<img src="/image/demo1.png" width = "50%" height = "50%" align=center />

<img src="/image/demo2.png" width = "50%" height = "50%" align=center />

## Introduction

This chapter describes scikit-learn from the software architecture perspective. Scikit-learn is an open source library for machine learning and data mining in Python language. Due to its efficiency and simplicity, it has been one of most popular machine learning tools. Besides, it can support many run-time environments. Considering its open-source property, the component of its developing member is complex. It’s necessary to do a software analysis for it.

### Power-interest grid

Figure 3 shows the quadrants of power and interest of scikit-learn stakeholders. The x-axis determines interest of stakeholders to scikit-learn which is divided into low and high interest. The interest of stakeholders is demonstrated by their willingness to explore, use, or contribute to the library. The y-axis determines the power of stakeholders which is also divide   into low and high power. Power is related to how influential the stakeholder is in scikit-learn's past, current, and future development. For example, the most important party of the stakeholders is the core developer team like in Figure1. They have responsibility for the project, so I think they should in upper-right area. On the other hand, students in Figure2, who had worded for the project, don’t work for the project in full time. I think they can be regarded as high-interest and low-power.

![1569843074193](C:\Users\14764\AppData\Roaming\Typora\typora-user-images\1569843074193.png)

<center>Figure 1</center>

![1569843236191](C:\Users\14764\AppData\Roaming\Typora\typora-user-images\1569843236191.png)

<center>Figure 2</center>

![1569843267194](C:\Users\14764\AppData\Roaming\Typora\typora-user-images\1569843267194.png)

<center>Figure 3</center>

## Stakeholders Analysis

### Core team members & contributors

In this section, According to Wikipedia,Stakeholder means an group,corporate,organization,member,or system that affects or can be affected by an organization’s actions. We explain it referred to Rozanski           and Woods' book and classified it into those categories:

#### Acquirers

Acquirer through certain procedures and means to obtain part or all of the ownership of an enterprise's investment behavior buyers can generally complete the acquisition through cash or stock acquired the actual control of the acquired enterprise international enterprise takeover is the result of transnational equity takeover or merger. So some enterprises wants to support or make an acquisition about scikit-learn and get benefit in return. Such as INRIA , Paris-Saclay Center for Data Science, NYU Moore-Sloan Data Science Environment, Télécom Paristech, Columbia University, Google, Tinyclues. 

#### Assessors

The reviewers evaluate whether the operation of the system meets the requirements and conforms to the law, and whether it is beneficial to the development of the system when the system needs to make changes, when scikit-learn faces some advice,they need to judge whether it suits the system’s functions and aim.

#### Communicators

They are able to absorb and express ideas in a variety of words with confidence and sensitivity, they are
responsible for explaining the functions of the scikit-learn to others and helping others install and operate the system. In the same time,they need to write and use documentation, official documentation, API documentation, etc. In the scikit-learn,There are several communication channels besides the website, such as Stack Overflow, a mailing list, and an IRC channel.

#### Developers

The developer is responsible for developing the complete scikit-learn from a vague concept,which refers to and include the people in GitHub to design,develop,release and maintain the system,who are founded by acquirers. They need to identify every detail of the software and fix any bugs to confirm the system can operate normally.

#### Maintainers

Maintainers is aimed at the daily maintenance of scikit-learn, to ensure the normal operation of software and emergency backup recovery and other measures like system maintenance, regular inspection, whether the software is normal operation, regular backup, clean up garbage information and so on. In scikit-learn,the job is mainly performed by Andreas Müller,with some others who are willing to maintain the documentation and code of the library in GitHub.

#### Production engineers

Product engineer generally belong to the category of technology department is responsible for product technical support, especially when new product development is generally with product engineer, the main task of the product engineer, is to make the process smooth and fluent, to ensure the production process of each gear tightly clasped and can make the products according to the schedule listed, even to shorten the time of design to production, strive for more market. In scikit-learn,Andreas Müller performs this role as a release manager. Staff in the companies who use scikit-learn can also fall under this,they are responsible for the work of design,deploy and manage the hardware including software environment.

#### Suppliers

The suppliers are responsible for providing the necessary hardware and software, as well as providing personnel to participate in development and maintenance as needed. In scikit-learn,some of the suppliers are:

- Rackspace: Provides cloud services for automatically building documentation.
- Shining Panda: Provides CPU time on continuous integration servers.
- GitHub: Provides hosting of version control
- Acquirers of project: Some provide funding to developers.

#### Support staffs

Support staffs are supposed to provide sufficient support to the normal operation of the system,at the same time,support staffs and communicators are both in the similar function and group in scikit-learn,they both need to help others understand the function of the system and help people use it. 

#### System administrator

System Administrator are mainly responsible for the whole system administrator related books system administrator related books network equipment and server system design installation configuration management and maintenance work, for the safe operation of the Intranet do technical support. Who also responsible for the daily management and maintenance of the specific information system, with the highest management authority of the information system. In scikit-learn,users will suppose that it will operate in any system,so system administrator are the people who are responsible for this.

#### Testers

The tester will continuously test the features on the eve of release to ensure the integrity and correctness of all features. In scikit-learn,testers will test the system to find whether the function is suitable for users to deployment,at the same time. When an new function or modification has been applied to the system,testers will use a high-coverage test to ensure they will perform well. Also some devices will help test the system without people’s work.

#### Users

For scikit-learn, users are those who operate the system in the very end, including some corporations and research team like Spotify, betaworks, evernote, booking.com, AWeber, YHat, INRIA,or some college team and some individual user with the aim of data analysis.

### Additional stakeholders

According to the categorization of stakeholders in the method proposed by Rozanski & Woods,But we can find many other stakeholders besides the main ones. Here are the some less mainly categories: contributors, users, funders and competitors.

#### Contributors

In scikit-learn,contributors means every one included the things about the system, a fairly large number of whom are those who are in GitHub and join the system. According to a data,in March 2017, there were 798 contributors on the GitHub repository of scikit-learn,many of them. They actively ask questions, participate in the development and maintenance of the system, and in addition to the main developers, they are the main supporters and developers of the system. To some extent, they also have a good understanding of the architecture, goals, attributes, and development prospects of the system.

#### Users

Unlike previous users, this includes everyone who USES the system, which includes the acquires above. On the one hand, they use the system to achieve their own purposes such as data analysis, on the other hand, they are also concerned about the existing problems and development prospects of the system. Just
the same as the contributors,To a certain extent, they also have an understanding of the integrity and usability of the program and are concerned about the simplicity and reusability of the code.

#### Funders

Funders are people who provide various kinds of support to the system. They can provide financial support in return for a certain amount of return. They can also provide the necessary hardware, software and platform for development. For example,individual funder may want to get the preferential use right or private right and some functions other users will not have. Company funder may want the profit percentage after the system has been released. And research group founder may want the develop group have some collaboration with their research projects. An example of this would be INRIA.

#### Competitors

For scikit-learn,competitors are those who work with the similar function system with scikit-learn, who wants to know the machine learning algorithm of it,on the one hand ,they want to know whom have the better algorithm which have a better market, on the other hand, they also wants to learn something their system doesn’t have. Examples of competitors are GraphLab
