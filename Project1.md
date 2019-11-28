## Imports


```python
import numpy as np
import os
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
```

## Reading Data


```python

dff = pd.read_csv('archive.csv')
df = dff.copy()
df.sample(5)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Year</th>
      <th>Category</th>
      <th>Prize</th>
      <th>Motivation</th>
      <th>Prize Share</th>
      <th>Laureate ID</th>
      <th>Laureate Type</th>
      <th>Full Name</th>
      <th>Birth Date</th>
      <th>Birth City</th>
      <th>Birth Country</th>
      <th>Sex</th>
      <th>Organization Name</th>
      <th>Organization City</th>
      <th>Organization Country</th>
      <th>Death Date</th>
      <th>Death City</th>
      <th>Death Country</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>364</th>
      <td>1963</td>
      <td>Medicine</td>
      <td>The Nobel Prize in Physiology or Medicine 1963</td>
      <td>"for their discoveries concerning the ionic me...</td>
      <td>1/3</td>
      <td>377</td>
      <td>Individual</td>
      <td>Andrew Fielding Huxley</td>
      <td>1917-11-22</td>
      <td>Hampstead</td>
      <td>United Kingdom</td>
      <td>Male</td>
      <td>University College</td>
      <td>London</td>
      <td>United Kingdom</td>
      <td>2012-05-30</td>
      <td>Grantchester</td>
      <td>United Kingdom</td>
    </tr>
    <tr>
      <th>915</th>
      <td>2013</td>
      <td>Chemistry</td>
      <td>The Nobel Prize in Chemistry 2013</td>
      <td>"for the development of multiscale models for ...</td>
      <td>1/3</td>
      <td>889</td>
      <td>Individual</td>
      <td>Martin Karplus</td>
      <td>1930-03-15</td>
      <td>Vienna</td>
      <td>Austria</td>
      <td>Male</td>
      <td>Harvard University</td>
      <td>Cambridge, MA</td>
      <td>United States of America</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>499</th>
      <td>1977</td>
      <td>Physics</td>
      <td>The Nobel Prize in Physics 1977</td>
      <td>"for their fundamental theoretical investigati...</td>
      <td>1/3</td>
      <td>107</td>
      <td>Individual</td>
      <td>Philip Warren Anderson</td>
      <td>1923-12-13</td>
      <td>Indianapolis, IN</td>
      <td>United States of America</td>
      <td>Male</td>
      <td>Bell Telephone Laboratories</td>
      <td>Murray Hill, NJ</td>
      <td>United States of America</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>661</th>
      <td>1994</td>
      <td>Chemistry</td>
      <td>The Nobel Prize in Chemistry 1994</td>
      <td>"for his contribution to carbocation chemistry"</td>
      <td>1/1</td>
      <td>280</td>
      <td>Individual</td>
      <td>George A. Olah</td>
      <td>1927-05-22</td>
      <td>Budapest</td>
      <td>Hungary</td>
      <td>Male</td>
      <td>University of Southern California</td>
      <td>Los Angeles, CA</td>
      <td>United States of America</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>300</th>
      <td>1955</td>
      <td>Literature</td>
      <td>The Nobel Prize in Literature 1955</td>
      <td>"for his vivid epic power which has renewed th...</td>
      <td>1/1</td>
      <td>626</td>
      <td>Individual</td>
      <td>Halldór Kiljan Laxness</td>
      <td>1902-04-23</td>
      <td>Reykjavik</td>
      <td>Iceland</td>
      <td>Male</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1998-02-08</td>
      <td>Reykjavik</td>
      <td>Iceland</td>
    </tr>
  </tbody>
</table>
</div>



## Data Exploration and Cleaning


```python
print("Shape\n%s Rows \n%s Columns " % (df.shape[0],df.shape[1]))
```

    Shape
    969 Rows 
    18 Columns 



```python
df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 969 entries, 0 to 968
    Data columns (total 18 columns):
    Year                    969 non-null int64
    Category                969 non-null object
    Prize                   969 non-null object
    Motivation              881 non-null object
    Prize Share             969 non-null object
    Laureate ID             969 non-null int64
    Laureate Type           969 non-null object
    Full Name               969 non-null object
    Birth Date              940 non-null object
    Birth City              941 non-null object
    Birth Country           943 non-null object
    Sex                     943 non-null object
    Organization Name       722 non-null object
    Organization City       716 non-null object
    Organization Country    716 non-null object
    Death Date              617 non-null object
    Death City              599 non-null object
    Death Country           605 non-null object
    dtypes: int64(2), object(16)
    memory usage: 136.3+ KB



```python
df.isnull().sum()
```




    Year                      0
    Category                  0
    Prize                     0
    Motivation               88
    Prize Share               0
    Laureate ID               0
    Laureate Type             0
    Full Name                 0
    Birth Date               29
    Birth City               28
    Birth Country            26
    Sex                      26
    Organization Name       247
    Organization City       253
    Organization Country    253
    Death Date              352
    Death City              370
    Death Country           364
    dtype: int64




```python
pd.unique(df["Laureate Type"])
```




    array(['Individual', 'Organization'], dtype=object)




```python
df[df["Laureate Type"] == "Organization"].shape
```




    (30, 18)




```python
df[df["Sex"].isnull()]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Year</th>
      <th>Category</th>
      <th>Prize</th>
      <th>Motivation</th>
      <th>Prize Share</th>
      <th>Laureate ID</th>
      <th>Laureate Type</th>
      <th>Full Name</th>
      <th>Birth Date</th>
      <th>Birth City</th>
      <th>Birth Country</th>
      <th>Sex</th>
      <th>Organization Name</th>
      <th>Organization City</th>
      <th>Organization Country</th>
      <th>Death Date</th>
      <th>Death City</th>
      <th>Death Country</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>24</th>
      <td>1904</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 1904</td>
      <td>NaN</td>
      <td>1/1</td>
      <td>467</td>
      <td>Organization</td>
      <td>Institut de droit international (Institute of ...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>61</th>
      <td>1910</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 1910</td>
      <td>NaN</td>
      <td>1/1</td>
      <td>477</td>
      <td>Organization</td>
      <td>Bureau international permanent de la Paix (Per...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>90</th>
      <td>1917</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 1917</td>
      <td>NaN</td>
      <td>1/1</td>
      <td>482</td>
      <td>Organization</td>
      <td>Comité international de la Croix Rouge (Intern...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>206</th>
      <td>1938</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 1938</td>
      <td>NaN</td>
      <td>1/1</td>
      <td>503</td>
      <td>Organization</td>
      <td>Office international Nansen pour les Réfugiés ...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>222</th>
      <td>1944</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 1944</td>
      <td>NaN</td>
      <td>1/1</td>
      <td>482</td>
      <td>Organization</td>
      <td>Comité international de la Croix Rouge (Intern...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>244</th>
      <td>1947</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 1947</td>
      <td>NaN</td>
      <td>1/2</td>
      <td>508</td>
      <td>Organization</td>
      <td>Friends Service Council (The Quakers)</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>245</th>
      <td>1947</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 1947</td>
      <td>NaN</td>
      <td>1/2</td>
      <td>509</td>
      <td>Organization</td>
      <td>American Friends Service Committee (The Quakers)</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>295</th>
      <td>1954</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 1954</td>
      <td>NaN</td>
      <td>1/1</td>
      <td>515</td>
      <td>Organization</td>
      <td>Office of the United Nations High Commissioner...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>365</th>
      <td>1963</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 1963</td>
      <td>NaN</td>
      <td>1/2</td>
      <td>482</td>
      <td>Organization</td>
      <td>Comité international de la Croix Rouge (Intern...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>366</th>
      <td>1963</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 1963</td>
      <td>NaN</td>
      <td>1/2</td>
      <td>523</td>
      <td>Organization</td>
      <td>Ligue des Sociétés de la Croix-Rouge (League o...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>383</th>
      <td>1965</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 1965</td>
      <td>NaN</td>
      <td>1/1</td>
      <td>525</td>
      <td>Organization</td>
      <td>United Nations Children's Fund (UNICEF)</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>416</th>
      <td>1969</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 1969</td>
      <td>NaN</td>
      <td>1/1</td>
      <td>527</td>
      <td>Organization</td>
      <td>International Labour Organization (I.L.O.)</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>498</th>
      <td>1977</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 1977</td>
      <td>NaN</td>
      <td>1/1</td>
      <td>537</td>
      <td>Organization</td>
      <td>Amnesty International</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>543</th>
      <td>1981</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 1981</td>
      <td>NaN</td>
      <td>1/1</td>
      <td>515</td>
      <td>Organization</td>
      <td>Office of the United Nations High Commissioner...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>578</th>
      <td>1985</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 1985</td>
      <td>NaN</td>
      <td>1/1</td>
      <td>547</td>
      <td>Organization</td>
      <td>International Physicians for the Prevention of...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>610</th>
      <td>1988</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 1988</td>
      <td>NaN</td>
      <td>1/1</td>
      <td>550</td>
      <td>Organization</td>
      <td>United Nations Peacekeeping Forces</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>682</th>
      <td>1995</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 1995</td>
      <td>"for their efforts to diminish the part played...</td>
      <td>1/2</td>
      <td>561</td>
      <td>Organization</td>
      <td>Pugwash Conferences on Science and World Affairs</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>705</th>
      <td>1997</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 1997</td>
      <td>"for their work for the banning and clearing o...</td>
      <td>1/2</td>
      <td>564</td>
      <td>Organization</td>
      <td>International Campaign to Ban Landmines (ICBL)</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>728</th>
      <td>1999</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 1999</td>
      <td>"in recognition of the organization's pioneeri...</td>
      <td>1/1</td>
      <td>568</td>
      <td>Organization</td>
      <td>Médecins Sans Frontières</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>754</th>
      <td>2001</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 2001</td>
      <td>"for their work for a better organized and mor...</td>
      <td>1/2</td>
      <td>748</td>
      <td>Organization</td>
      <td>United Nations (U.N.)</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>808</th>
      <td>2005</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 2005</td>
      <td>"for their efforts to prevent nuclear energy f...</td>
      <td>1/2</td>
      <td>797</td>
      <td>Organization</td>
      <td>International Atomic Energy Agency (IAEA)</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>821</th>
      <td>2006</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 2006</td>
      <td>"for their efforts to create economic and soci...</td>
      <td>1/2</td>
      <td>810</td>
      <td>Organization</td>
      <td>Grameen Bank</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>833</th>
      <td>2007</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 2007</td>
      <td>"for their efforts to build up and disseminate...</td>
      <td>1/2</td>
      <td>818</td>
      <td>Organization</td>
      <td>Intergovernmental Panel on Climate Change (IPCC)</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>909</th>
      <td>2012</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 2012</td>
      <td>"for over six decades contributed to the advan...</td>
      <td>1/1</td>
      <td>881</td>
      <td>Organization</td>
      <td>European Union (EU)</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>927</th>
      <td>2013</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 2013</td>
      <td>"for its extensive efforts to eliminate chemic...</td>
      <td>1/1</td>
      <td>893</td>
      <td>Organization</td>
      <td>Organisation for the Prohibition of Chemical W...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>955</th>
      <td>2015</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 2015</td>
      <td>"for its decisive contribution to the building...</td>
      <td>1/1</td>
      <td>925</td>
      <td>Organization</td>
      <td>National Dialogue Quartet</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>




```python
#df[df["Prize Share"] == "1/2"]
df[df["Full Name"] == "Paul Ehrlich"]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Year</th>
      <th>Category</th>
      <th>Prize</th>
      <th>Motivation</th>
      <th>Prize Share</th>
      <th>Laureate ID</th>
      <th>Laureate Type</th>
      <th>Full Name</th>
      <th>Birth Date</th>
      <th>Birth City</th>
      <th>Birth Country</th>
      <th>Sex</th>
      <th>Organization Name</th>
      <th>Organization City</th>
      <th>Organization Country</th>
      <th>Death Date</th>
      <th>Death City</th>
      <th>Death Country</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>46</th>
      <td>1908</td>
      <td>Medicine</td>
      <td>The Nobel Prize in Physiology or Medicine 1908</td>
      <td>"in recognition of their work on immunity"</td>
      <td>1/2</td>
      <td>302</td>
      <td>Individual</td>
      <td>Paul Ehrlich</td>
      <td>1854-03-14</td>
      <td>Strehlen (Strzelin)</td>
      <td>Prussia (Poland)</td>
      <td>Male</td>
      <td>Goettingen University</td>
      <td>Göttingen</td>
      <td>Germany</td>
      <td>1915-08-20</td>
      <td>Bad Homburg vor der Höhe</td>
      <td>Germany</td>
    </tr>
    <tr>
      <th>47</th>
      <td>1908</td>
      <td>Medicine</td>
      <td>The Nobel Prize in Physiology or Medicine 1908</td>
      <td>"in recognition of their work on immunity"</td>
      <td>1/2</td>
      <td>302</td>
      <td>Individual</td>
      <td>Paul Ehrlich</td>
      <td>1854-03-14</td>
      <td>Strehlen (Strzelin)</td>
      <td>Prussia (Poland)</td>
      <td>Male</td>
      <td>Königliches Institut für experimentelle Therap...</td>
      <td>Frankfurt-on-the-Main</td>
      <td>Germany</td>
      <td>1915-08-20</td>
      <td>Bad Homburg vor der Höhe</td>
      <td>Germany</td>
    </tr>
  </tbody>
</table>
</div>




```python

#df[df["Organization Name"].isnull()]
l = df[df["Organization Name"].isnull()]
k = l[l["Laureate Type"] == "Organization"]
print("Shape\n%s Rows \n%s Columns " % (k.shape[0],k.shape[1]))

```

    Shape
    30 Rows 
    18 Columns 


## Cleaning 

### Removing Duplicates 


```python
print(len(df))
df.drop_duplicates(subset = "Laureate ID",keep = "first", inplace = True)
print(len(df))
df[df["Full Name"] == "Paul Ehrlich"]
```

    969
    904





<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Year</th>
      <th>Category</th>
      <th>Prize</th>
      <th>Motivation</th>
      <th>Prize Share</th>
      <th>Laureate ID</th>
      <th>Laureate Type</th>
      <th>Full Name</th>
      <th>Birth Date</th>
      <th>Birth City</th>
      <th>Birth Country</th>
      <th>Sex</th>
      <th>Organization Name</th>
      <th>Organization City</th>
      <th>Organization Country</th>
      <th>Death Date</th>
      <th>Death City</th>
      <th>Death Country</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>46</th>
      <td>1908</td>
      <td>Medicine</td>
      <td>The Nobel Prize in Physiology or Medicine 1908</td>
      <td>"in recognition of their work on immunity"</td>
      <td>1/2</td>
      <td>302</td>
      <td>Individual</td>
      <td>Paul Ehrlich</td>
      <td>1854-03-14</td>
      <td>Strehlen (Strzelin)</td>
      <td>Prussia (Poland)</td>
      <td>Male</td>
      <td>Goettingen University</td>
      <td>Göttingen</td>
      <td>Germany</td>
      <td>1915-08-20</td>
      <td>Bad Homburg vor der Höhe</td>
      <td>Germany</td>
    </tr>
  </tbody>
</table>
</div>



### Deleting Motivation values


```python
del df['Motivation']
df.sample(5)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Year</th>
      <th>Category</th>
      <th>Prize</th>
      <th>Prize Share</th>
      <th>Laureate ID</th>
      <th>Laureate Type</th>
      <th>Full Name</th>
      <th>Birth Date</th>
      <th>Birth City</th>
      <th>Birth Country</th>
      <th>Sex</th>
      <th>Organization Name</th>
      <th>Organization City</th>
      <th>Organization Country</th>
      <th>Death Date</th>
      <th>Death City</th>
      <th>Death Country</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>626</th>
      <td>1990</td>
      <td>Economics</td>
      <td>The Sveriges Riksbank Prize in Economic Scienc...</td>
      <td>1/3</td>
      <td>705</td>
      <td>Individual</td>
      <td>Merton H. Miller</td>
      <td>1923-05-16</td>
      <td>Boston, MA</td>
      <td>United States of America</td>
      <td>Male</td>
      <td>University of Chicago</td>
      <td>Chicago, IL</td>
      <td>United States of America</td>
      <td>2000-06-03</td>
      <td>Chicago, IL</td>
      <td>United States of America</td>
    </tr>
    <tr>
      <th>408</th>
      <td>1969</td>
      <td>Chemistry</td>
      <td>The Nobel Prize in Chemistry 1969</td>
      <td>1/2</td>
      <td>237</td>
      <td>Individual</td>
      <td>Derek H. R. Barton</td>
      <td>1918-09-08</td>
      <td>Gravesend</td>
      <td>United Kingdom</td>
      <td>Male</td>
      <td>Imperial College</td>
      <td>London</td>
      <td>United Kingdom</td>
      <td>1998-03-16</td>
      <td>College Station, TX</td>
      <td>United States of America</td>
    </tr>
    <tr>
      <th>531</th>
      <td>1980</td>
      <td>Medicine</td>
      <td>The Nobel Prize in Physiology or Medicine 1980</td>
      <td>1/3</td>
      <td>420</td>
      <td>Individual</td>
      <td>Jean Dausset</td>
      <td>1916-10-19</td>
      <td>Toulouse</td>
      <td>France</td>
      <td>Male</td>
      <td>Université de Paris, Laboratoire Immuno-Hémato...</td>
      <td>Paris</td>
      <td>France</td>
      <td>2009-06-06</td>
      <td>Palma, Majorca</td>
      <td>Spain</td>
    </tr>
    <tr>
      <th>416</th>
      <td>1969</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 1969</td>
      <td>1/1</td>
      <td>527</td>
      <td>Organization</td>
      <td>International Labour Organization (I.L.O.)</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>955</th>
      <td>2015</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 2015</td>
      <td>1/1</td>
      <td>925</td>
      <td>Organization</td>
      <td>National Dialogue Quartet</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



### Fixing Incorrect Data of Laureate Type ( Org -> Individual )


```python

#df.loc[df["Sex"].isnull(), "Sex"] = "Org"

mask = (df["Laureate Type"] == "Organization") & (df["Birth Date"].notnull())
df["Laureate Type"][mask] = "Individual"

l = df[df["Laureate Type"] == "Organization"]
k = l[l["Birth Date"].notnull()]
k
```

    /Users/mac/anaconda3/lib/python3.7/site-packages/ipykernel_launcher.py:4: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      after removing the cwd from sys.path.





<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Year</th>
      <th>Category</th>
      <th>Prize</th>
      <th>Prize Share</th>
      <th>Laureate ID</th>
      <th>Laureate Type</th>
      <th>Full Name</th>
      <th>Birth Date</th>
      <th>Birth City</th>
      <th>Birth Country</th>
      <th>Sex</th>
      <th>Organization Name</th>
      <th>Organization City</th>
      <th>Organization Country</th>
      <th>Death Date</th>
      <th>Death City</th>
      <th>Death Country</th>
    </tr>
  </thead>
  <tbody>
  </tbody>
</table>
</div>




```python
k = df[df["Laureate Type"] == "Organization"]
print("Shape\n%s Rows \n%s Columns " % (k.shape[0],k.shape[1]))
k
#df.isnull().sum()
```

    Shape
    26 Rows 
    17 Columns 





<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Year</th>
      <th>Category</th>
      <th>Prize</th>
      <th>Prize Share</th>
      <th>Laureate ID</th>
      <th>Laureate Type</th>
      <th>Full Name</th>
      <th>Birth Date</th>
      <th>Birth City</th>
      <th>Birth Country</th>
      <th>Sex</th>
      <th>Organization Name</th>
      <th>Organization City</th>
      <th>Organization Country</th>
      <th>Death Date</th>
      <th>Death City</th>
      <th>Death Country</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>24</th>
      <td>1904</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 1904</td>
      <td>1/1</td>
      <td>467</td>
      <td>Organization</td>
      <td>Institut de droit international (Institute of ...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>61</th>
      <td>1910</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 1910</td>
      <td>1/1</td>
      <td>477</td>
      <td>Organization</td>
      <td>Bureau international permanent de la Paix (Per...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>90</th>
      <td>1917</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 1917</td>
      <td>1/1</td>
      <td>482</td>
      <td>Organization</td>
      <td>Comité international de la Croix Rouge (Intern...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>206</th>
      <td>1938</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 1938</td>
      <td>1/1</td>
      <td>503</td>
      <td>Organization</td>
      <td>Office international Nansen pour les Réfugiés ...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>222</th>
      <td>1944</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 1944</td>
      <td>1/1</td>
      <td>482</td>
      <td>Organization</td>
      <td>Comité international de la Croix Rouge (Intern...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>244</th>
      <td>1947</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 1947</td>
      <td>1/2</td>
      <td>508</td>
      <td>Organization</td>
      <td>Friends Service Council (The Quakers)</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>245</th>
      <td>1947</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 1947</td>
      <td>1/2</td>
      <td>509</td>
      <td>Organization</td>
      <td>American Friends Service Committee (The Quakers)</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>295</th>
      <td>1954</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 1954</td>
      <td>1/1</td>
      <td>515</td>
      <td>Organization</td>
      <td>Office of the United Nations High Commissioner...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>365</th>
      <td>1963</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 1963</td>
      <td>1/2</td>
      <td>482</td>
      <td>Organization</td>
      <td>Comité international de la Croix Rouge (Intern...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>366</th>
      <td>1963</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 1963</td>
      <td>1/2</td>
      <td>523</td>
      <td>Organization</td>
      <td>Ligue des Sociétés de la Croix-Rouge (League o...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>383</th>
      <td>1965</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 1965</td>
      <td>1/1</td>
      <td>525</td>
      <td>Organization</td>
      <td>United Nations Children's Fund (UNICEF)</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>416</th>
      <td>1969</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 1969</td>
      <td>1/1</td>
      <td>527</td>
      <td>Organization</td>
      <td>International Labour Organization (I.L.O.)</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>498</th>
      <td>1977</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 1977</td>
      <td>1/1</td>
      <td>537</td>
      <td>Organization</td>
      <td>Amnesty International</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>543</th>
      <td>1981</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 1981</td>
      <td>1/1</td>
      <td>515</td>
      <td>Organization</td>
      <td>Office of the United Nations High Commissioner...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>578</th>
      <td>1985</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 1985</td>
      <td>1/1</td>
      <td>547</td>
      <td>Organization</td>
      <td>International Physicians for the Prevention of...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>610</th>
      <td>1988</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 1988</td>
      <td>1/1</td>
      <td>550</td>
      <td>Organization</td>
      <td>United Nations Peacekeeping Forces</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>682</th>
      <td>1995</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 1995</td>
      <td>1/2</td>
      <td>561</td>
      <td>Organization</td>
      <td>Pugwash Conferences on Science and World Affairs</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>705</th>
      <td>1997</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 1997</td>
      <td>1/2</td>
      <td>564</td>
      <td>Organization</td>
      <td>International Campaign to Ban Landmines (ICBL)</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>728</th>
      <td>1999</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 1999</td>
      <td>1/1</td>
      <td>568</td>
      <td>Organization</td>
      <td>Médecins Sans Frontières</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>754</th>
      <td>2001</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 2001</td>
      <td>1/2</td>
      <td>748</td>
      <td>Organization</td>
      <td>United Nations (U.N.)</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>808</th>
      <td>2005</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 2005</td>
      <td>1/2</td>
      <td>797</td>
      <td>Organization</td>
      <td>International Atomic Energy Agency (IAEA)</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>821</th>
      <td>2006</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 2006</td>
      <td>1/2</td>
      <td>810</td>
      <td>Organization</td>
      <td>Grameen Bank</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>833</th>
      <td>2007</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 2007</td>
      <td>1/2</td>
      <td>818</td>
      <td>Organization</td>
      <td>Intergovernmental Panel on Climate Change (IPCC)</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>909</th>
      <td>2012</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 2012</td>
      <td>1/1</td>
      <td>881</td>
      <td>Organization</td>
      <td>European Union (EU)</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>927</th>
      <td>2013</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 2013</td>
      <td>1/1</td>
      <td>893</td>
      <td>Organization</td>
      <td>Organisation for the Prohibition of Chemical W...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>955</th>
      <td>2015</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 2015</td>
      <td>1/1</td>
      <td>925</td>
      <td>Organization</td>
      <td>National Dialogue Quartet</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



### Dropping organization since they only represent 2.5% of the dataframe ( 23 rows out of 904 rows)


```python
print("Shape\n%s Rows \n%s Columns " % (df.shape[0],df.shape[1]))
df = df[df["Laureate Type"] == "Individual"]
print("Shape\n%s Rows \n%s Columns " % (df.shape[0],df.shape[1]))
df[df["Laureate Type"] == "Individual"]
df.isnull().sum()
#df[df["Death Date"].isnull()]
```

    Shape
    969 Rows 
    17 Columns 
    Shape
    943 Rows 
    17 Columns 





    Year                      0
    Category                  0
    Prize                     0
    Prize Share               0
    Laureate ID               0
    Laureate Type             0
    Full Name                 0
    Birth Date                3
    Birth City                2
    Birth Country             0
    Sex                       0
    Organization Name       221
    Organization City       227
    Organization Country    227
    Death Date              326
    Death City              344
    Death Country           338
    dtype: int64



### Dropping the Death City, Death Date, Death Country as they will not affect Data visualisation in any way


```python
df = df.drop("Death Date", axis=1)
df = df.drop("Death City", axis=1)
df = df.drop("Death Country", axis=1)
df.isnull().sum()

```




    Year                      0
    Category                  0
    Prize                     0
    Prize Share               0
    Laureate ID               0
    Laureate Type             0
    Full Name                 0
    Birth Date                3
    Birth City                2
    Birth Country             0
    Sex                       0
    Organization Name       221
    Organization City       227
    Organization Country    227
    dtype: int64




### Adding missing birth date and city for individuals 


```python
df.loc[df["Laureate ID"] == 841, "Birth Date"] = "1952-04-01" #venk
df.loc[df["Laureate ID"] == 864, "Birth Date"] = "1959-09-22" #saul
df.loc[df["Laureate ID"] == 747, "Birth City"] = "Chaguanas" # sir Vidiadhar
df.loc[df["Laureate ID"] == 855, "Birth City"] = "Changchun" # liu Xiaobo
df.isnull().sum()
```




    Year                      0
    Category                  0
    Prize                     0
    Prize Share               0
    Laureate ID               0
    Laureate Type             0
    Full Name                 0
    Birth Date                0
    Birth City                0
    Birth Country             0
    Sex                       0
    Organization Name         0
    Organization City         0
    Organization Country    213
    dtype: int64




```python

df.loc[df["Laureate ID"] == 6, "Organization Country"] = "self" 
df.loc[df["Laureate ID"] == 6, "Organization City"] = "self" 
df.loc[df["Laureate ID"] == 6, "Organization Name"] = "self" 

df.loc[df["Laureate ID"] == 318, "Organization Country"] = "Tunisia" 

df.loc[df["Laureate ID"] == 684, "Organization Country"] = "self" 
df.loc[df["Laureate ID"] == 684, "Organization City"] = "self" 
df.loc[df["Laureate ID"] == 684, "Organization Name"] = "self" 

df.loc[df["Laureate ID"] == 685, "Organization Country"] = "self" 
df.loc[df["Laureate ID"] == 685, "Organization City"] = "self" 
df.loc[df["Laureate ID"] == 685, "Organization Name"] = "self" 

df.loc[df["Laureate ID"] == 270, "Organization Country"] = "USA" 
df.loc[df["Laureate ID"] == 270, "Organization City"] = "Maryland" 

df.loc[df["Laureate ID"] == 461, "Organization Country"] = "USA" 
df.loc[df["Laureate ID"] == 461, "Organization City"] = "Maryland" 

df.loc[df["Laureate ID"] == 770, "Organization Country"] = "USA" 
df.loc[df["Laureate ID"] == 770, "Organization City"] = "Maryland"

df.loc[df["Laureate ID"] == 811, "Organization Country"] = "USA" 
df.loc[df["Laureate ID"] == 811, "Organization City"] = "Maryland"

df.loc[df["Laureate ID"] == 831, "Organization Country"] = "USA" 
df.loc[df["Laureate ID"] == 831, "Organization City"] = "Maryland" 

df.loc[df["Laureate ID"] == 842, "Organization Country"] = "USA" 
df.loc[df["Laureate ID"] == 842, "Organization City"] = "Maryland" 

df.loc[df["Laureate ID"] == 837, "Organization Country"] = "USA" 
df.loc[df["Laureate ID"] == 837, "Organization City"] = "Maryland" 

df.loc[df["Laureate ID"] == 878, "Organization Country"] = "USA" 
df.loc[df["Laureate ID"] == 878, "Organization City"] = "Maryland" 

df.loc[df["Laureate ID"] == 885, "Organization Country"] = "USA" 
df.loc[df["Laureate ID"] == 885, "Organization City"] = "Maryland" 

df.loc[df["Laureate ID"] == 886, "Organization Country"] = "USA" 
df.loc[df["Laureate ID"] == 886, "Organization City"] = "Maryland" 

k = df[df["Organization Country"].isnull()]
l = k[k["Category"] != "Peace"]
l[l["Category"] != "Literature"]

df[df["Organization Country"].isnull()].shape


# ba3d keda el hytba22a meen b2a? el peace wl literature bas
# homa el wa7ideen el hyb2o be null, then momken terunno el 3 lines el ta7t
#dool be el text el t7boha, take care el order yb2a keda 3shan law 3mlto 
#3 lines el ta7t dool homa el nas el physics wl mediccine msh ht3rfo tgebeehom tany

df.loc[df["Organization Name"].isnull(), "Organization Name"] = "Self"
df.loc[df["Organization City"].isnull(), "Organization City"] = "Self"
df.loc[df["Organization Country"].isnull(), "Organization Country"] = "Self"

df.isnull().sum()

```




    Year                    0
    Category                0
    Prize                   0
    Prize Share             0
    Laureate ID             0
    Laureate Type           0
    Full Name               0
    Birth Date              0
    Birth City              0
    Birth Country           0
    Sex                     0
    Organization Name       0
    Organization City       0
    Organization Country    0
    dtype: int64




```python
df.to_csv('archiveData_Cleaned.csv')
```


```python

```
