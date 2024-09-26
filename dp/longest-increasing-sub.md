## 概要

https://leetcode.com/problems/longest-increasing-subsequence/

## Intuition

## Complexity

## Code

dpを使ったアルゴリズムはO(N*2)かかる

```cpp
class Solution {
public:
       int lengthOfLIS(vector<int>& nums) {
            if(nums.empty()) return 0;
            int size = nums.size();
            vector<int> dp(size,1);
            for(int i = 1; i < size; i++) {
                for (int j = 0; j < i; j++) {
                    if(nums[j]<nums[i]) {
                       dp[i] = max(dp[i], dp[j] + 1);
                    }
                }
            }
        return *max_element(dp.begin(), dp.end());
    }
};
```

上書きしていくロジック(lower_bound)だと、O(N log N)になる。

```cpp
class Solution {
public:
       int lengthOfLIS(vector<int>& nums) {
           vector<int> sub;
           for(int x: nums) {
              // if empty or last element is lower than x;
            if(sub.empty() || sub[sub.size() -1] < x) {
                sub.push_back(x);
            } else {
              // switch x with first lower than x element in a sub
             auto it = lower_bound(sub.begin(), sub.end(), x);
             *it = x;
            }
           }
           return sub.size();
    }
};
```

- `lower_bound` finds the first element in `sub` that is greater than or equal to `x`.
- `it = x` replaces that element with `x`.Last login: Sun Sep 22 13:32:41 on ttys000
subaru@wajis-MacBook-Air ~ % ls
Applications
CertificateSigningRequest.certSigningRequest
Desktop
Dev
Documents
Downloads
Library
Movies
Music
Pictures
Postman
Postman Agent
Public
cloud-builders-community
development
go
subaru@wajis-MacBook-Air ~ % mkdir development/tiD
B
subaru@wajis-MacBook-Air ~ % mkdir first-pj
subaru@wajis-MacBook-Air ~ % cd first-pj
subaru@wajis-MacBook-Air first-pj % git init
Initialized empty Git repository in /Users/subaru/first-pj/.git/
subaru@wajis-MacBook-Air first-pj % gs
On branch main

No commits yet

nothing to commit (create/copy files and use "git add" to track)
subaru@wajis-MacBook-Air first-pj % >....
          LAST_VALUE(record_date) OVER (PARTITION BY stock_symbol ORDER BY record_date) AS last_day,
          NTH_VALUE(close, 1) OVER (PARTITION BY stock_symbol ORDER BY record_date DESC) AS last_close_price,
          NTH_VALUE(close, 2) OVER (PARTITION BY stock_symbol ORDER BY record_date DESC) AS last_2nd_close_price,
          ROW_NUMBER() OVER (PARTITION BY stock_symbol ORDER BY record_date DESC) AS row_num
        FROM stock_price_history sph
        WHERE
          record_date > DATE_SUB((SELECT MAX(record_date) FROM stock_price_history), INTERVAL 2 DAY)
          AND sph.stock_symbol IN (SELECT stock_symbol FROM index_companies ic)
      ) sub
      WHERE
          row_num = 2
  )
  SELECT
      ic.stock_symbol, ic.exchange_symbol, ic.short_name, ic.weight, ic.market_cap, ic.revenue_growth,
    clp.last_change, clp.last_change_day, clp.last_close_price, clp.last_2nd_close_price, clp.last_change_percentage
  FROM index_companies ic
  LEFT JOIN companies_latest_price clp ON ic.stock_symbol = clp.stock_symbol
  ORDER BY ic.weight DESC;
  """

  execQuery(query, queryName)

except mysql.connector.Error as err:
  print(f"Error: {err}")

finally:
  # Closing the connection
  if conn.is_connected():
      cursor.close()
      conn.close()
      print("MySQL connection is closed")
EOF
subaru@wajis-MacBook-Air first-pj % ls
test01.py
subaru@wajis-MacBook-Air first-pj % cat test01.py
import mysql.connector
import datetime
from prettytable import PrettyTable

config = {
  'user': '3ZVcAAgkrezsmQn.uX9f37wl',
  'password': 'BKGvp9U4uk5i9U2qpSyEsN9cPUK+muw47gD0',
  'host': 'gateway01.ap-northeast-1.prod.aws.tidbcloud.com',
  'database': 'sp500insight',
  'raise_on_warnings': False,
  'port': 4000,
}

def execQuery(query, queryName):
  print(queryName)
  print("クエリ実行開始時間：", datetime.datetime.now().strftime("%H:%M:%S.%f"))
  # Executing the query
  cursor.execute(query)
  print("クエリ実行終了時間：", datetime.datetime.now().strftime("%H:%M:%S.%f"))

  # Fetching all rows from the last executed query
  results = cursor.fetchall()

  # Creating a PrettyTable object
  my_table = PrettyTable()

  # Adding field names (column names) to the table
  field_names = [i[0] for i in cursor.description]
  my_table.field_names = field_names

  # Adding rows to the table
  for row in results:
      my_table.add_row(row)

  # Printing the table
  print(my_table)
  print("==========================")

try:
  # Establishing the connection
  conn = mysql.connector.connect(**config)

  # Creating a cursor object
  cursor = conn.cursor()

  # Your SQL query
  queryName= "■■■クエリ①：業界別時価総額■■■"
  query = """
  SELECT
      `sector`,
      SUM(`market_cap`) AS `total_market_cap`
  FROM
      `companies`
  GROUP BY
      `sector`;
  """
  execQuery(query, queryName)


  queryName= "■■■クエリ②：エネルギー業界での時価総額ランキング■■■"
  query = """
  WITH index_companies AS (
      SELECT
          c.stock_symbol, c.short_name, c.exchange_symbol, ic.weight, c.market_cap AS market_cap, c.revenue_growth AS revenue_growth
      FROM companies c
      JOIN index_compositions ic ON c.stock_symbol = ic.stock_symbol
      WHERE index_symbol = 'SP500' AND sector = 'Energy'
  ), companies_latest_price AS (
      SELECT
          stock_symbol, DATE(last_day) AS last_change_day, last_close_price, last_2nd_close_price,
               last_close_price - last_2nd_close_price                          AS last_change,
    (last_close_price - last_2nd_close_price) / last_2nd_close_price AS last_change_percentage
      FROM (
        SELECT
          LAST_VALUE(stock_symbol) OVER (PARTITION BY stock_symbol ORDER BY record_date) AS stock_symbol,
          LAST_VALUE(record_date) OVER (PARTITION BY stock_symbol ORDER BY record_date) AS last_day,
          NTH_VALUE(close, 1) OVER (PARTITION BY stock_symbol ORDER BY record_date DESC) AS last_close_price,
          NTH_VALUE(close, 2) OVER (PARTITION BY stock_symbol ORDER BY record_date DESC) AS last_2nd_close_price,
          ROW_NUMBER() OVER (PARTITION BY stock_symbol ORDER BY record_date DESC) AS row_num
        FROM stock_price_history sph
        WHERE
          record_date > DATE_SUB((SELECT MAX(record_date) FROM stock_price_history), INTERVAL 2 DAY)
          AND sph.stock_symbol IN (SELECT stock_symbol FROM index_companies ic)
      ) sub
      WHERE
          row_num = 2
  )
  SELECT
      ic.stock_symbol, ic.exchange_symbol, ic.short_name, ic.weight, ic.market_cap, ic.revenue_growth,
    clp.last_change, clp.last_change_day, clp.last_close_price, clp.last_2nd_close_price, clp.last_change_percentage
  FROM index_companies ic
  LEFT JOIN companies_latest_price clp ON ic.stock_symbol = clp.stock_symbol
  ORDER BY ic.weight DESC;
  """

  execQuery(query, queryName)

except mysql.connector.Error as err:
  print(f"Error: {err}")

finally:
  # Closing the connection
  if conn.is_connected():
      cursor.close()
      conn.close()
      print("MySQL connection is closed")
subaru@wajis-MacBook-Air first-pj % python test01.py
zsh: command not found: python
subaru@wajis-MacBook-Air first-pj % python3 test01.py
Traceback (most recent call last):
  File "/Users/subaru/first-pj/test01.py", line 1, in <module>
    import mysql.connector
ModuleNotFoundError: No module named 'mysql'
subaru@wajis-MacBook-Air first-pj % pip3 install mysql-connector-python

Collecting mysql-connector-python
  Obtaining dependency information for mysql-connector-python from https://files.pythonhosted.org/packages/17/0c/c0fa64f15fde771a477fd7d3977f8587c5a17db32ea6f4e59a87098c8196/mysql_connector_python-9.0.0-cp312-cp312-macosx_13_0_arm64.whl.metadata
  Downloading mysql_connector_python-9.0.0-cp312-cp312-macosx_13_0_arm64.whl.metadata (2.0 kB)
Downloading mysql_connector_python-9.0.0-cp312-cp312-macosx_13_0_arm64.whl (13.4 MB)
   ━━━━━━━━━━━━ 13.4/13.4 MB 21.4 MB/s eta 0:00:00
Installing collected packages: mysql-connector-python
Successfully installed mysql-connector-python-9.0.0

[notice] A new release of pip is available: 23.2.1 -> 24.2
[notice] To update, run: pip3 install --upgrade pip
subaru@wajis-MacBook-Air first-pj % python3 test01.py
Traceback (most recent call last):
  File "/Users/subaru/first-pj/test01.py", line 3, in <module>
    from prettytable import PrettyTable
ModuleNotFoundError: No module named 'prettytable'
subaru@wajis-MacBook-Air first-pj % python3 -m pip show mysql-connector-python

Name: mysql-connector-python
Version: 9.0.0
Summary: MySQL driver written in Python
Home-page: http://dev.mysql.com/doc/connector-python/en/index.html
Author: Oracle and/or its affiliates
Author-email:
License: GNU GPLv2 (with FOSS License Exception)
Location: /Library/Frameworks/Python.framework/Versions/3.12/lib/python3.12/site-packages
Requires:
Required-by:
subaru@wajis-MacBook-Air first-pj % python3 test01.py

Traceback (most recent call last):
  File "/Users/subaru/first-pj/test01.py", line 3, in <module>
    from prettytable import PrettyTable
ModuleNotFoundError: No module named 'prettytable'
subaru@wajis-MacBook-Air first-pj % pip3 install prettytable

Collecting prettytable
  Obtaining dependency information for prettytable from https://files.pythonhosted.org/packages/d9/5a/bfdc26c0e19156992b1dc9de47f0b2e8992fe43db9981d814f860bdce2b3/prettytable-3.11.0-py3-none-any.whl.metadata
  Downloading prettytable-3.11.0-py3-none-any.whl.metadata (30 kB)
Collecting wcwidth (from prettytable)
  Obtaining dependency information for wcwidth from https://files.pythonhosted.org/packages/fd/84/fd2ba7aafacbad3c4201d395674fc6348826569da3c0937e75505ead3528/wcwidth-0.2.13-py2.py3-none-any.whl.metadata
  Downloading wcwidth-0.2.13-py2.py3-none-any.whl.metadata (14 kB)
Downloading prettytable-3.11.0-py3-none-any.whl (28 kB)
Downloading wcwidth-0.2.13-py2.py3-none-any.whl (34 kB)
Installing collected packages: wcwidth, prettytable
Successfully installed prettytable-3.11.0 wcwidth-0.2.13

[notice] A new release of pip is available: 23.2.1 -> 24.2
[notice] To update, run: pip3 install --upgrade pip
subaru@wajis-MacBook-Air first-pj % python3 test01.py

■■■クエリ①：業界別時価総額■■■
クエリ実行開始時間： 13:57:58.943312
クエリ実行終了時間： 13:57:58.959455
+------------------------+------------------+
|         sector         | total_market_cap |
+------------------------+------------------+
|      Real Estate       |   927820086272   |
|    Basic Materials     |   889871172608   |
|         Energy         |  1788549832704   |
|   Consumer Defensive   |  2872367655424   |
| Communication Services |  3669841019392   |
|      Industrials       |  3199253085184   |
|   Consumer Cyclical    |  3238477340160   |
|       Healthcare       |  5122903213056   |
|       Utilities        |  1023797270016   |
|       Technology       |  7906855563776   |
|   Financial Services   |  4661122020864   |
+------------------------+------------------+
==========================
■■■クエリ②：エネルギー業界での時価総額ランキング■■■
クエリ実行開始時間： 13:57:58.965639
クエリ実行終了時間： 13:58:00.193242
+--------------+-----------------+---------------------------------+------------------------+--------------+----------------+---------------------+-----------------+----------------------+----------------------+-------------------------+
| stock_symbol | exchange_symbol |            short_name           |         weight         |  market_cap  | revenue_growth |     last_change     | last_change_day |   last_close_price   | last_2nd_close_price |  last_change_percentage |
+--------------+-----------------+---------------------------------+------------------------+--------------+----------------+---------------------+-----------------+----------------------+----------------------+-------------------------+
|     XOM      |       NYQ       |     Exxon Mobil Corporation     |  0.013441937585551282  | 474511933440 |     0.689      |  1.2800064086914100 |    2023-01-10   | 111.3700027465820300 | 110.0899963378906200 |  0.01162690935843785832 |
|     CVX      |       NYQ       |       Chevron Corporation       |  0.009567226003564158  | 337731289088 |      0.81      | -0.8399963378906000 |    2023-01-10   | 175.1999969482422000 | 176.0399932861328000 | -0.00477162218772232275 |
|     COP      |       NYQ       |          ConocoPhillips         |  0.004417814517419704  | 155952644096 |     1.237      |  0.1599960327148400 |    2023-01-10   | 118.2399978637695300 | 118.0800018310546900 |  0.00135497993084178221 |
|     SLB      |       NYQ       |        Schlumberger N.V.        | 0.0022377960040345308  | 78996119552  |     0.279      |         None        |       None      |         None         |         None         |           None          |
|     EOG      |       NYQ       |       EOG Resources, Inc.       | 0.0021024604924476343  | 74218659840  |     0.924      | -0.7600021362304600 |    2023-01-10   | 126.8499984741211000 | 127.6100006103515600 | -0.00595566282105957140 |
|     MPC      |       NYQ       |  Marathon Petroleum Corporation |  0.002050747487098508  | 72393146368  |     0.818      |  1.0999984741211000 |    2023-01-10   | 117.5999984741211000 | 116.5000000000000000 |  0.00944204698816394850 |
|     OXY      |       NYQ       | Occidental Petroleum Corporatio |  0.001695676548826279  | 59858837504  |     0.792      |         None        |       None      |         None         |         None         |           None          |
|     PXD      |       NYQ       | Pioneer Natural Resources Compa | 0.0016017013932190817  | 56541433856  |     0.641      |         None        |       None      |         None         |         None         |           None          |
|     VLO      |       NYQ       |    Valero Energy Corporation    | 0.0015472850935959574  | 54620491776  |     0.593      |         None        |       None      |         None         |         None         |           None          |
|     HES      |       NYQ       |         Hess Corporation        | 0.0012744122628788822  | 44987846656  |     0.776      |  1.6300048828125000 |    2023-01-10   | 145.2500000000000000 | 143.6199951171875000 |  0.01134942861878312162 |
|     PSX      |       NYQ       |           Phillips 66           | 0.0012589621320069535  | 44442443776  |     0.799      |         None        |       None      |         None         |         None         |           None          |
|     KMI      |       NYQ       |       Kinder Morgan, Inc.       | 0.0012062691273686955  | 42582335488  |     0.354      |  0.0400009155273440 |    2023-01-10   | 18.7800006866455080  | 18.7399997711181640  |  0.00213452059850037319 |
|     DVN      |       NYQ       |     Devon Energy Corporation    | 0.0012053375130788024  | 42549448704  |     0.789      | -0.2800025939941400 |    2023-01-10   | 62.8499984741210940  | 63.1300010681152340  | -0.00443533326875800678 |
|     WMB      |       NYQ       |  Williams Companies, Inc. (The) | 0.0011162172177908748  | 39403425792  |     0.091      |         None        |       None      |         None         |         None         |           None          |
|     HAL      |       NYQ       |       Halliburton Company       | 0.0010393744130051477  | 36690808832  |     0.388      |  0.0500030517578160 |    2023-01-10   | 40.9900016784668000  | 40.9399986267089840  |  0.00122137404580161222 |
|     OKE      |       NYQ       |           ONEOK, Inc.           | 0.0008854096904464799  | 31255721984  |     0.769      |         None        |       None      |         None         |         None         |           None          |
|     BKR      |       NMS       |       Baker Hughes Company      | 0.0008807471517968449  | 31091130368  |     0.054      | -0.0400009155273450 |    2023-01-10   | 30.8299999237060550  | 30.8700008392334000  | -0.00129578602008027511 |
|     FANG     |       NMS       |     Diamondback Energy, Inc.    |  0.000730020047064916  | 25770334208  |     0.565      |  0.2900085449218600 |    2023-01-10   | 142.2400054931640600 | 141.9499969482422000 |  0.00204303311839874783 |
|     MRO      |       NYQ       |     Marathon Oil Corporation    | 0.0005965698099807201  | 21059426304  |     0.731      |  0.0399990081787110 |    2023-01-10   | 27.0499992370605470  | 27.0100002288818360  |  0.00148089625471161593 |
|     CTRA     |       NYQ       |       Coterra Energy Inc.       | 0.0005690298110137097  | 20087240704  |     5.204      | -0.3899993896484380 |    2023-01-10   | 24.6900005340576170  | 25.0799999237060550  | -0.01555021494556719473 |
|     TRGP     |       NYQ       |      Targa Resources, Inc.      | 0.0004842024080652878  | 17092760576  |     0.773      |         None        |       None      |         None         |         None         |           None          |
|     APA      |       NMS       |         APA Corporation         | 0.00039910236086756015 | 14088655872  |     0.374      |  0.5799980163574200 |    2023-01-10   | 43.8199996948242200  | 43.2400016784668000  |  0.01341345961709929139 |
|     EQT      |       NYQ       |         EQT Corporation         | 0.0003576031445812937  | 12623697920  |     1.092      | -0.6399993896484380 |    2023-01-10   | 33.4000015258789060  | 34.0400009155273440  | -0.01880139166966068805 |
+--------------+-----------------+---------------------------------+------------------------+--------------+----------------+---------------------+-----------------+----------------------+----------------------+-------------------------+
==========================
MySQL connection is closed
subaru@wajis-MacBook-Air first-pj % gs
On branch main

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	test01.py

nothing added to commit but untracked files present (use "git add" to track)
subaru@wajis-MacBook-Air first-pj % cat test01.py
import mysql.connector
import datetime
from prettytable import PrettyTable

config = {
  'user': '3ZVcAAgkrezsmQn.uX9f37wl',
  'password': 'BKGvp9U4uk5i9U2qpSyEsN9cPUK+muw47gD0',
  'host': 'gateway01.ap-northeast-1.prod.aws.tidbcloud.com',
  'database': 'sp500insight',
  'raise_on_warnings': False,
  'port': 4000,
}

def execQuery(query, queryName):
  print(queryName)
  print("クエリ実行開始時間：", datetime.datetime.now().strftime("%H:%M:%S.%f"))
  # Executing the query
  cursor.execute(query)
  print("クエリ実行終了時間：", datetime.datetime.now().strftime("%H:%M:%S.%f"))

  # Fetching all rows from the last executed query
  results = cursor.fetchall()

  # Creating a PrettyTable object
  my_table = PrettyTable()

  # Adding field names (column names) to the table
  field_names = [i[0] for i in cursor.description]
  my_table.field_names = field_names

  # Adding rows to the table
  for row in results:
      my_table.add_row(row)

  # Printing the table
  print(my_table)
  print("==========================")

try:
  # Establishing the connection
  conn = mysql.connector.connect(**config)

  # Creating a cursor object
  cursor = conn.cursor()

  # Your SQL query
  queryName= "■■■クエリ①：業界別時価総額■■■"
  query = """
  SELECT
      `sector`,
      SUM(`market_cap`) AS `total_market_cap`
  FROM
      `companies`
  GROUP BY
      `sector`;
  """
  execQuery(query, queryName)


  queryName= "■■■クエリ②：エネルギー業界での時価総額ランキング■■■"
  query = """
  WITH index_companies AS (
      SELECT
          c.stock_symbol, c.short_name, c.exchange_symbol, ic.weight, c.market_cap AS market_cap, c.revenue_growth AS revenue_growth
      FROM companies c
      JOIN index_compositions ic ON c.stock_symbol = ic.stock_symbol
      WHERE index_symbol = 'SP500' AND sector = 'Energy'
  ), companies_latest_price AS (
      SELECT
          stock_symbol, DATE(last_day) AS last_change_day, last_close_price, last_2nd_close_price,
               last_close_price - last_2nd_close_price                          AS last_change,
    (last_close_price - last_2nd_close_price) / last_2nd_close_price AS last_change_percentage
      FROM (
        SELECT
          LAST_VALUE(stock_symbol) OVER (PARTITION BY stock_symbol ORDER BY record_date) AS stock_symbol,
          LAST_VALUE(record_date) OVER (PARTITION BY stock_symbol ORDER BY record_date) AS last_day,
          NTH_VALUE(close, 1) OVER (PARTITION BY stock_symbol ORDER BY record_date DESC) AS last_close_price,
          NTH_VALUE(close, 2) OVER (PARTITION BY stock_symbol ORDER BY record_date DESC) AS last_2nd_close_price,
          ROW_NUMBER() OVER (PARTITION BY stock_symbol ORDER BY record_date DESC) AS row_num
        FROM stock_price_history sph
        WHERE
          record_date > DATE_SUB((SELECT MAX(record_date) FROM stock_price_history), INTERVAL 2 DAY)
          AND sph.stock_symbol IN (SELECT stock_symbol FROM index_companies ic)
      ) sub
      WHERE
          row_num = 2
  )
  SELECT
      ic.stock_symbol, ic.exchange_symbol, ic.short_name, ic.weight, ic.market_cap, ic.revenue_growth,
    clp.last_change, clp.last_change_day, clp.last_close_price, clp.last_2nd_close_price, clp.last_change_percentage
  FROM index_companies ic
  LEFT JOIN companies_latest_price clp ON ic.stock_symbol = clp.stock_symbol
  ORDER BY ic.weight DESC;
  """

  execQuery(query, queryName)

except mysql.connector.Error as err:
  print(f"Error: {err}")

finally:
  # Closing the connection
  if conn.is_connected():
      cursor.close()
      conn.close()
      print("MySQL connection is closed")
subaru@wajis-MacBook-Air first-pj % python test01.py
zsh: command not found: python
subaru@wajis-MacBook-Air first-pj % python3 test01.py
■■■クエリ①：業界別時価総額■■■
クエリ実行開始時間： 14:13:23.981619
クエリ実行終了時間： 14:13:24.018184
+------------------------+------------------+
|         sector         | total_market_cap |
+------------------------+------------------+
|   Consumer Cyclical    |  3238477340160   |
|      Industrials       |  3199253085184   |
|      Real Estate       |   927820086272   |
|       Healthcare       |  5122903213056   |
|       Technology       |  7906855563776   |
|   Financial Services   |  4661122020864   |
|       Utilities        |  1023797270016   |
|         Energy         |  1788549832704   |
|    Basic Materials     |   889871172608   |
|   Consumer Defensive   |  2872367655424   |
| Communication Services |  3669841019392   |
+------------------------+------------------+
==========================
■■■クエリ②：エネルギー業界での時価総額ランキング■■■
クエリ実行開始時間： 14:13:24.023077
クエリ実行終了時間： 14:13:24.170770
+--------------+-----------------+---------------------------------+------------------------+--------------+----------------+---------------------+-----------------+----------------------+----------------------+-------------------------+
| stock_symbol | exchange_symbol |            short_name           |         weight         |  market_cap  | revenue_growth |     last_change     | last_change_day |   last_close_price   | last_2nd_close_price |  last_change_percentage |
+--------------+-----------------+---------------------------------+------------------------+--------------+----------------+---------------------+-----------------+----------------------+----------------------+-------------------------+
|     XOM      |       NYQ       |     Exxon Mobil Corporation     |  0.013441937585551282  | 474511933440 |     0.689      |  1.2800064086914100 |    2023-01-10   | 111.3700027465820300 | 110.0899963378906200 |  0.01162690935843785832 |
|     CVX      |       NYQ       |       Chevron Corporation       |  0.009567226003564158  | 337731289088 |      0.81      | -0.8399963378906000 |    2023-01-10   | 175.1999969482422000 | 176.0399932861328000 | -0.00477162218772232275 |
|     COP      |       NYQ       |          ConocoPhillips         |  0.004417814517419704  | 155952644096 |     1.237      |  0.1599960327148400 |    2023-01-10   | 118.2399978637695300 | 118.0800018310546900 |  0.00135497993084178221 |
|     SLB      |       NYQ       |        Schlumberger N.V.        | 0.0022377960040345308  | 78996119552  |     0.279      |         None        |       None      |         None         |         None         |           None          |
|     EOG      |       NYQ       |       EOG Resources, Inc.       | 0.0021024604924476343  | 74218659840  |     0.924      | -0.7600021362304600 |    2023-01-10   | 126.8499984741211000 | 127.6100006103515600 | -0.00595566282105957140 |
|     MPC      |       NYQ       |  Marathon Petroleum Corporation |  0.002050747487098508  | 72393146368  |     0.818      |  1.0999984741211000 |    2023-01-10   | 117.5999984741211000 | 116.5000000000000000 |  0.00944204698816394850 |
|     OXY      |       NYQ       | Occidental Petroleum Corporatio |  0.001695676548826279  | 59858837504  |     0.792      |         None        |       None      |         None         |         None         |           None          |
|     PXD      |       NYQ       | Pioneer Natural Resources Compa | 0.0016017013932190817  | 56541433856  |     0.641      |         None        |       None      |         None         |         None         |           None          |
|     VLO      |       NYQ       |    Valero Energy Corporation    | 0.0015472850935959574  | 54620491776  |     0.593      |         None        |       None      |         None         |         None         |           None          |
|     HES      |       NYQ       |         Hess Corporation        | 0.0012744122628788822  | 44987846656  |     0.776      |  1.6300048828125000 |    2023-01-10   | 145.2500000000000000 | 143.6199951171875000 |  0.01134942861878312162 |
|     PSX      |       NYQ       |           Phillips 66           | 0.0012589621320069535  | 44442443776  |     0.799      |         None        |       None      |         None         |         None         |           None          |
|     KMI      |       NYQ       |       Kinder Morgan, Inc.       | 0.0012062691273686955  | 42582335488  |     0.354      |  0.0400009155273440 |    2023-01-10   | 18.7800006866455080  | 18.7399997711181640  |  0.00213452059850037319 |
|     DVN      |       NYQ       |     Devon Energy Corporation    | 0.0012053375130788024  | 42549448704  |     0.789      | -0.2800025939941400 |    2023-01-10   | 62.8499984741210940  | 63.1300010681152340  | -0.00443533326875800678 |
|     WMB      |       NYQ       |  Williams Companies, Inc. (The) | 0.0011162172177908748  | 39403425792  |     0.091      |         None        |       None      |         None         |         None         |           None          |
|     HAL      |       NYQ       |       Halliburton Company       | 0.0010393744130051477  | 36690808832  |     0.388      |  0.0500030517578160 |    2023-01-10   | 40.9900016784668000  | 40.9399986267089840  |  0.00122137404580161222 |
|     OKE      |       NYQ       |           ONEOK, Inc.           | 0.0008854096904464799  | 31255721984  |     0.769      |         None        |       None      |         None         |         None         |           None          |
|     BKR      |       NMS       |       Baker Hughes Company      | 0.0008807471517968449  | 31091130368  |     0.054      | -0.0400009155273450 |    2023-01-10   | 30.8299999237060550  | 30.8700008392334000  | -0.00129578602008027511 |
|     FANG     |       NMS       |     Diamondback Energy, Inc.    |  0.000730020047064916  | 25770334208  |     0.565      |  0.2900085449218600 |    2023-01-10   | 142.2400054931640600 | 141.9499969482422000 |  0.00204303311839874783 |
|     MRO      |       NYQ       |     Marathon Oil Corporation    | 0.0005965698099807201  | 21059426304  |     0.731      |  0.0399990081787110 |    2023-01-10   | 27.0499992370605470  | 27.0100002288818360  |  0.00148089625471161593 |
|     CTRA     |       NYQ       |       Coterra Energy Inc.       | 0.0005690298110137097  | 20087240704  |     5.204      | -0.3899993896484380 |    2023-01-10   | 24.6900005340576170  | 25.0799999237060550  | -0.01555021494556719473 |
|     TRGP     |       NYQ       |      Targa Resources, Inc.      | 0.0004842024080652878  | 17092760576  |     0.773      |         None        |       None      |         None         |         None         |           None          |
|     APA      |       NMS       |         APA Corporation         | 0.00039910236086756015 | 14088655872  |     0.374      |  0.5799980163574200 |    2023-01-10   | 43.8199996948242200  | 43.2400016784668000  |  0.01341345961709929139 |
|     EQT      |       NYQ       |         EQT Corporation         | 0.0003576031445812937  | 12623697920  |     1.092      | -0.6399993896484380 |    2023-01-10   | 33.4000015258789060  | 34.0400009155273440  | -0.01880139166966068805 |
+--------------+-----------------+---------------------------------+------------------------+--------------+----------------+---------------------+-----------------+----------------------+----------------------+-------------------------+
==========================
MySQL connection is closed
subaru@wajis-MacBook-Air first-pj %
subaru@wajis-MacBook-Air first-pj %
subaru@wajis-MacBook-Air first-pj %
subaru@wajis-MacBook-Air first-pj %
subaru@wajis-MacBook-Air first-pj %
subaru@wajis-MacBook-Air first-pj %
subaru@wajis-MacBook-Air first-pj %
subaru@wajis-MacBook-Air first-pj %
subaru@wajis-MacBook-Air first-pj % ls
test01.py
subaru@wajis-MacBook-Air first-pj % mv ~/Downloads/ai-exec3.zip .
subaru@wajis-MacBook-Air first-pj % ls
ai-exec3.zip	test01.py
subaru@wajis-MacBook-Air first-pj % unzip ai-exec3.zip
Archive:  ai-exec3.zip
   creating: ai-exec3/
  inflating: ai-exec3/semantic_search_test.llama_index_rag_test-schema.sql
  inflating: ai-exec3/semantic_search_test-schema-create.sql
  inflating: ai-exec3/semantic_search_test.llama_index_rag_test.000000000.sql
  inflating: ai-exec3/metadata
subaru@wajis-MacBook-Air first-pj % ls
ai-exec3	ai-exec3.zip	test01.py
subaru@wajis-MacBook-Air first-pj % rm ai-exec3.zip
subaru@wajis-MacBook-Air first-pj % ls
ai-exec3	test01.py
subaru@wajis-MacBook-Air first-pj % cd ai-exec3
subaru@wajis-MacBook-Air ai-exec3 % la
zsh: command not found: la
subaru@wajis-MacBook-Air ai-exec3 % ls
metadata
semantic_search_test-schema-create.sql
semantic_search_test.llama_index_rag_test-schema.sql
semantic_search_test.llama_index_rag_test.000000000.sql
subaru@wajis-MacBook-Air ai-exec3 % mv ~/Downloads/build-graph.py ~/Downloads/test-graph.py ~/Downloads/tidb-chat.py ~/Downloads/tidb-chat-v2.py .
subaru@wajis-MacBook-Air ai-exec3 % ls
build-graph.py
metadata
semantic_search_test-schema-create.sql
semantic_search_test.llama_index_rag_test-schema.sql
semantic_search_test.llama_index_rag_test.000000000.sql
test-graph.py
tidb-chat-v2.py
tidb-chat.py
subaru@wajis-MacBook-Air ai-exec3 % cursor .
subaru@wajis-MacBook-Air ai-exec3 % history
 1516  cat test01.py
 1517  python test01.py
 1518  python3 test01.py
 1519  ls
 1520  mv ~/Downloads/ai-exec3.zip .
 1521  ls
 1522  unzip ai-exec3.zip
 1523  ls
 1524  rm ai-exec3.zip
 1525  ls
 1526  cd ai-exec3
 1527  la
 1528  ls
 1529  mv ~/Downloads/build-graph.py ~/Downloads/test-graph.py ~/Downloads/tidb-chat.py ~/Downloads/tidb-chat-v2.py .
 1530  ls
 1531  cursor .

vector[root+1]がrightである。
右が常にrootより大きい

preorder traversal [9, 3, 15, 20, 7, 10, 39, 1, 94]
          /
             \
```

## Code

#include <unordered_map>
#include <vector>
    TreeNode *right;
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
            inorderMap[inorder[i]] = i;
        return buildTreeHelper(preorder, 0, preorder.size() - 1, 0, inorder.size() - 1);
    }
            return nullptr;
subaru@wajis-MacBook-Air ai-exec3 %
subaru@wajis-MacBook-Air ai-exec3 %
subaru@wajis-MacBook-Air ai-exec3 %
subaru@wajis-MacBook-Air ai-exec3 %
subaru@wajis-MacBook-Air ai-exec3 %
subaru@wajis-MacBook-Air ai-exec3 %
subaru@wajis-MacBook-Air ai-exec3 % cd ..
subaru@wajis-MacBook-Air first-pj % cd ..
subaru@wajis-MacBook-Air ~ % ls
Applications					Pictures
CertificateSigningRequest.certSigningRequest	Postman
Desktop						Postman Agent
Dev						Public
Documents					cloud-builders-community
Downloads					development
Library						first-pj
Movies						go
Music
subaru@wajis-MacBook-Air ~ % cd development
subaru@wajis-MacBook-Air development % ls
AUTHORS				agg				go-server
CODEOWNERS			analysis_options.yaml		hugo-blog
CODE_OF_CONDUCT.md		bin				k8s-fastly
CONTRIBUTING.md			complier_c_flat			lambdaTest
LICENSE				dartdoc_options.yaml		movie_merge
PATENT_GRANT			dev				node-dst
README.md			dstChecker			npmPackages
TESTOWNERS			examples			packages
TracksForPrivateAthleteDB	flutter				stanford-algorithms-go
Wanpo				flutter_console.bat		tiDB
ZerokenAPI			flutter_macos_3.16.7-stable.zip	version
subaru@wajis-MacBook-Air development % cd ..
subaru@wajis-MacBook-Air ~ % ls
Applications					Pictures
CertificateSigningRequest.certSigningRequest	Postman
Desktop						Postman Agent
Dev						Public
Documents					cloud-builders-community
Downloads					development
Library						first-pj
Movies						go
Music
subaru@wajis-MacBook-Air ~ % cd Dev/arai60
## 概要
subaru@wajis-MacBook-Air arai60 %
subaru@wajis-MacBook-Air arai60 % ls
CppBasic.md	bfs		dfs		heap		stack
README.md	binary-tree	hash-map	linked-list
subaru@wajis-MacBook-Air arai60 % vim binary-tree/pre-in-order-traversal.md
subaru@wajis-MacBook-Air arai60 % g
zsh: command not found: g
subaru@wajis-MacBook-Air arai60 % gs
On branch main
Your branch is up to date with 'origin/main'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	modified:   binary-tree/pre-in-order-traversal.md

no changes added to commit (use "git add" and/or "git commit -a")
subaru@wajis-MacBook-Air arai60 % ga .
subaru@wajis-MacBook-Air arai60 % gs
On branch main
Your branch is up to date with 'origin/main'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
	modified:   binary-tree/pre-in-order-traversal.md

subaru@wajis-MacBook-Air arai60 % gcm "finish pre-inorder-traversal"
[main e155196] finish pre-inorder-traversal
 1 file changed, 127 insertions(+)
subaru@wajis-MacBook-Air arai60 % gps
Enumerating objects: 7, done.
Counting objects: 100% (7/7), done.
Delta compression using up to 8 threads
## Intuition

## Complexity

## Code

dpを使ったアルゴリズムはO(N*2)かかる

```cpp
class Solution {
public:
       int lengthOfLIS(vector<int>& nums) {
            if(nums.empty()) return 0;
            int size = nums.size();
            vector<int> dp(size,1);
            for(int i = 1; i < size; i++) {
                for (int j = 0; j < i; j++) {
                    if(nums[j]<nums[i]) {
                       dp[i] = max(dp[i], dp[j] + 1);
                    }
                }
            }
        return *max_element(dp.begin(), dp.end());
    }
};
```

上書きしていくロジック(lower_bound)だと、O(N log N)になる。

```cpp
class Solution {
public:
       int lengthOfLIS(vector<int>& nums) {
           vector<int> sub;
           for(int x: nums) {
              // if empty or last element is lower than x;
            if(sub.empty() || sub[sub.size() -1] < x) {
                sub.push_back(x);
            } else {
              // switch x with first lower than x element in a sub
             auto it = lower_bound(sub.begin(), sub.end(), x);
             *it = x;
            }
           }
           return sub.size();
    }
};
```

- `lower_bound` finds the first element in `sub` that is greater than or equal to `x`.
- `it = x` replaces that element with `x`.## 概要
