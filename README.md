# Jupyter-Data-manipulation
Using python libraries to clean and process the data
#Step1
import pandas as pd
import numpy as np
f = pd.read_excel("/home/PLN9630/FA12_Layout.xlsx")
f_short = f.iloc[6676:6895]
f_short["ID-Name"].unique()

#Step2
import pandas as pd

f = pd.read_excel("/home/PLN9630/FA12_Layout.xlsx")

f["Name"] = f["ID-Name"].rename()

widths = f.col_len.tolist()

names = f.Name.tolist()
#We will start with all 7 possible answers:
use_cols = [u"v1-ID",u"v6677-I'M 1ST OF FRNDS HAVE NEW ELCTRNC EQUIP", u"v6694-I'M 1ST OF FRNDS HAVE NEW ELCTRNC EQUIP",
            u"v6711-I'M 1ST OF FRNDS HAVE NEW ELCTRNC EQUIP",
            u"v6728-I'M 1ST OF FRNDS HAVE NEW ELCTRNC EQUIP", 
            u"v6745-I'M 1ST OF FRNDS HAVE NEW ELCTRNC EQUIP",
            u"v6762-I'M 1ST OF FRNDS HAVE NEW ELCTRNC EQUIP",
            u"v6779-I'M 1ST OF FRNDS HAVE NEW ELCTRNC EQUIP",
           ]
tx = pd.read_fwf("/home/PLN9630/first1k.txt", widths=widths, names=names, usecols = use_cols)

tx[np.isnan(tx)] = 0
tx.head(10)
v1-ID	v6677-I'M 1ST OF FRNDS HAVE NEW ELCTRNC EQUIP	v6694-I'M 1ST OF FRNDS HAVE NEW ELCTRNC EQUIP	v6711-I'M 1ST OF FRNDS HAVE NEW ELCTRNC EQUIP	v6728-I'M 1ST OF FRNDS HAVE NEW ELCTRNC EQUIP	v6745-I'M 1ST OF FRNDS HAVE NEW ELCTRNC EQUIP	v6762-I'M 1ST OF FRNDS HAVE NEW ELCTRNC EQUIP	v6779-I'M 1ST OF FRNDS HAVE NEW ELCTRNC EQUIP
0	1985951	0.0	0.0	0.0	1.0	0.0	0.0	0.0
1	1985952	0.0	0.0	0.0	1.0	0.0	0.0	0.0
2	1985954	0.0	0.0	0.0	0.0	0.0	1.0	1.0
3	1985955	0.0	0.0	0.0	0.0	0.0	1.0	1.0
4	1985960	0.0	0.0	0.0	0.0	0.0	1.0	1.0
5	1985961	0.0	0.0	0.0	0.0	0.0	0.0	0.0
6	1985963	0.0	0.0	0.0	0.0	0.0	1.0	1.0
7	1985964	0.0	0.0	0.0	0.0	1.0	0.0	1.0
8	1985966	0.0	0.0	0.0	0.0	1.0	0.0	1.0
9	1985967	0.0	0.0	0.0	0.0	0.0	1.0	1.0
#Step3
tx.shape
#(1000, 8)
#We have a thousand survey partcipants, which means we would expect to see each row to be sum up as one. If it's not equal to 1, 
#it's either was omitted or more than one option has been selected by a respondent.

#Let's rename the column titles to make it easier to understand the table.
txt = tx.rename(columns={u"v6677-I'M 1ST OF FRNDS HAVE NEW ELCTRNC EQUIP": "Agree A Lot-I'M 1ST OF FRNDS HAVE NEW ELCTRNC EQUIP", 
                         u"v6694-I'M 1ST OF FRNDS HAVE NEW ELCTRNC EQUIP": "Agree A Little-I'M 1ST OF FRNDS HAVE NEW ELCTRNC EQUIP",
            u"v6711-I'M 1ST OF FRNDS HAVE NEW ELCTRNC EQUIP": "Any Agree-I'M 1ST OF FRNDS HAVE NEW ELCTRNC EQUIP",
            u"v6728-I'M 1ST OF FRNDS HAVE NEW ELCTRNC EQUIP": "Neither Agree Or Disagree-I'M 1ST OF FRNDS HAVE NEW ELCTRNC EQUIP", 
            u"v6745-I'M 1ST OF FRNDS HAVE NEW ELCTRNC EQUIP": "Disagree A Little-I'M 1ST OF FRNDS HAVE NEW ELCTRNC EQUIP",
            u"v6762-I'M 1ST OF FRNDS HAVE NEW ELCTRNC EQUIP": "Disagre A Lot-I'M 1ST OF FRNDS HAVE NEW ELCTRNC EQUIP",
            u"v6779-I'M 1ST OF FRNDS HAVE NEW ELCTRNC EQUIP": "Any Disagree-I'M 1ST OF FRNDS HAVE NEW ELCTRNC EQUIP"})

#As we may see, keeping such columns as Any Agree or Any Disagree creates a duplication and possible further confusion. 
#One of potential possibilities would be to drop them alltogether and leave the rest. It would be worth considering keeping
#those variables in case when we would observe missed responses. They could have been potentially used a replacement.

#Let's go ahead and create an extra column to tally up the sum values for each row
txt.assign(Sum=tx.select_dtypes(['number']).sum(1) - tx["v1-ID"])
#As we may see, respondents were very attentive: there are no duplicated answers.
v1-ID	Agree A Lot-I'M 1ST OF FRNDS HAVE NEW ELCTRNC EQUIP	Agree A Little-I'M 1ST OF FRNDS HAVE NEW ELCTRNC EQUIP	Any Agree-I'M 1ST OF FRNDS HAVE NEW ELCTRNC EQUIP	Neither Agree Or Disagree-I'M 1ST OF FRNDS HAVE NEW ELCTRNC EQUIP	Disagree A Little-I'M 1ST OF FRNDS HAVE NEW ELCTRNC EQUIP	Disagre A Lot-I'M 1ST OF FRNDS HAVE NEW ELCTRNC EQUIP	Any Disagree-I'M 1ST OF FRNDS HAVE NEW ELCTRNC EQUIP	Sum
0	1985951	0.0	0.0	0.0	1.0	0.0	0.0	0.0	1.0
1	1985952	0.0	0.0	0.0	1.0	0.0	0.0	0.0	1.0
2	1985954	0.0	0.0	0.0	0.0	0.0	1.0	1.0	2.0
3	1985955	0.0	0.0	0.0	0.0	0.0	1.0	1.0	2.0
4	1985960	0.0	0.0	0.0	0.0	0.0	1.0	1.0	2.0
5	1985961	0.0	0.0	0.0	0.0	0.0	0.0	0.0	0.0
6	1985963	0.0	0.0	0.0	0.0	0.0	1.0	1.0	2.0
7	1985964	0.0	0.0	0.0	0.0	1.0	0.0	1.0	2.0
8	1985966	0.0	0.0	0.0	0.0	1.0	0.0	1.0	2.0
9	1985967	0.0	0.0	0.0	0.0	0.0	1.0	1.0	2.0
10	1985969	0.0	0.0	0.0	0.0	0.0	1.0	1.0	2.0
11	1985970	0.0	1.0	1.0	0.0	0.0	0.0	0.0	2.0
12	1985977	0.0	0.0	0.0	0.0	0.0	1.0	1.0	2.0
13	1985979	0.0	0.0	0.0	0.0	0.0	0.0	0.0	0.0
14	1985980	0.0	0.0	0.0	0.0	0.0	1.0	1.0	2.0
15	1985981	0.0	0.0	0.0	0.0	0.0	0.0	0.0	0.0
16	1985986	0.0	0.0	0.0	0.0	1.0	0.0	1.0	2.0
17	1985987	0.0	0.0	0.0	0.0	0.0	1.0	1.0	2.0
18	1985989	0.0	0.0	0.0	0.0	1.0	0.0	1.0	2.0
19	1985990	0.0	0.0	0.0	1.0	0.0	0.0	0.0	1.0
20	1985992	0.0	0.0	0.0	0.0	0.0	0.0	0.0	0.0
21	1985994	0.0	0.0	0.0	1.0	0.0	0.0	0.0	1.0
22	1985995	0.0	1.0	1.0	0.0	0.0	0.0	0.0	2.0
23	1985997	0.0	0.0	0.0	0.0	1.0	0.0	1.0	2.0
24	1985998	0.0	0.0	0.0	0.0	0.0	1.0	1.0	2.0
25	1986022	0.0	0.0	0.0	0.0	0.0	1.0	1.0	2.0
26	1986023	0.0	0.0	0.0	1.0	0.0	0.0	0.0	1.0
27	1986027	0.0	0.0	0.0	0.0	0.0	1.0	1.0	2.0
28	1986030	0.0	0.0	0.0	0.0	0.0	1.0	1.0	2.0
29	1986031	0.0	0.0	0.0	0.0	0.0	1.0	1.0	2.0
...	...	...	...	...	...	...	...	...	...
970	1990174	0.0	0.0	0.0	1.0	0.0	0.0	0.0	1.0
971	1990187	0.0	0.0	0.0	1.0	0.0	0.0	0.0	1.0
972	1990188	0.0	0.0	0.0	1.0	0.0	0.0	0.0	1.0
973	1990210	0.0	0.0	0.0	0.0	0.0	1.0	1.0	2.0
974	1990211	0.0	0.0	0.0	0.0	0.0	1.0	1.0	2.0
975	1990213	0.0	0.0	0.0	0.0	1.0	0.0	1.0	2.0
976	1990214	0.0	0.0	0.0	0.0	1.0	0.0	1.0	2.0
977	1990234	0.0	0.0	0.0	0.0	0.0	1.0	1.0	2.0
978	1990235	0.0	0.0	0.0	0.0	0.0	1.0	1.0	2.0
979	1990237	0.0	1.0	1.0	0.0	0.0	0.0	0.0	2.0
980	1990238	0.0	0.0	0.0	0.0	0.0	1.0	1.0	2.0
981	1990239	0.0	0.0	0.0	0.0	0.0	1.0	1.0	2.0
982	1990240	0.0	0.0	0.0	0.0	0.0	1.0	1.0	2.0
983	1990245	0.0	0.0	0.0	0.0	0.0	1.0	1.0	2.0
984	1990246	0.0	0.0	0.0	0.0	0.0	1.0	1.0	2.0
985	1990253	0.0	0.0	0.0	0.0	1.0	0.0	1.0	2.0
986	1990258	0.0	0.0	0.0	0.0	1.0	0.0	1.0	2.0
987	1990259	0.0	0.0	0.0	0.0	0.0	1.0	1.0	2.0
988	1990269	0.0	0.0	0.0	1.0	0.0	0.0	0.0	1.0
989	1990275	0.0	0.0	0.0	0.0	0.0	1.0	1.0	2.0
990	1990276	0.0	0.0	0.0	1.0	0.0	0.0	0.0	1.0
991	1990282	0.0	0.0	0.0	0.0	0.0	1.0	1.0	2.0
992	1990308	0.0	0.0	0.0	0.0	1.0	0.0	1.0	2.0
993	1990310	0.0	0.0	0.0	1.0	0.0	0.0	0.0	1.0
994	1990311	0.0	0.0	0.0	1.0	0.0	0.0	0.0	1.0
995	1990318	0.0	0.0	0.0	0.0	1.0	0.0	1.0	2.0
996	1990319	0.0	0.0	0.0	0.0	1.0	0.0	1.0	2.0
997	1990339	0.0	1.0	1.0	0.0	0.0	0.0	0.0	2.0
998	1990341	0.0	0.0	0.0	1.0	0.0	0.0	0.0	1.0
999	1990342	0.0	0.0	0.0	0.0	1.0	0.0	1.0	2.0
1000 rows × 9 columns

#Repeat Step2
#Let's update the dataset by removing "Any Disagree" and "Any Agree" values
f = pd.read_excel("/home/PLN9630/FA12_Layout.xlsx")

f["Name"] = f["ID-Name"].rename()


widths = f.col_len.tolist()

names = f.Name.tolist()

use_cols = [u"v1-ID",u"v6677-I'M 1ST OF FRNDS HAVE NEW ELCTRNC EQUIP", u"v6694-I'M 1ST OF FRNDS HAVE NEW ELCTRNC EQUIP",
            u"v6728-I'M 1ST OF FRNDS HAVE NEW ELCTRNC EQUIP", 
            u"v6745-I'M 1ST OF FRNDS HAVE NEW ELCTRNC EQUIP",
            u"v6762-I'M 1ST OF FRNDS HAVE NEW ELCTRNC EQUIP"
           ]
# we selcted 5 options for the answer : "Agree A Lot", Agree A Little", "Neither Agree or Disagree", "Disagree A Little", "Disagree A Lot"
tx = pd.read_fwf("/home/PLN9630/first1k.txt", widths=widths, names=names, usecols = use_cols)

tx[np.isnan(tx)] = 0
tx.head(10)

#We have a thousand survey partcipants, which means we would expect to see each row to be sum up as one. If it's not equal to 1, 
#it's either was omitted or more than one option has been selected by a respondent.
txt1 = tx.rename(columns={u"v6677-I'M 1ST OF FRNDS HAVE NEW ELCTRNC EQUIP": "Agree A Lot-I'M 1ST OF FRNDS HAVE NEW ELCTRNC EQUIP", 
                         u"v6694-I'M 1ST OF FRNDS HAVE NEW ELCTRNC EQUIP": "Agree A Little-I'M 1ST OF FRNDS HAVE NEW ELCTRNC EQUIP",
            u"v6728-I'M 1ST OF FRNDS HAVE NEW ELCTRNC EQUIP": "Neither Agree Or Disagree-I'M 1ST OF FRNDS HAVE NEW ELCTRNC EQUIP", 
            u"v6745-I'M 1ST OF FRNDS HAVE NEW ELCTRNC EQUIP": "Disagree A Little-I'M 1ST OF FRNDS HAVE NEW ELCTRNC EQUIP",
            u"v6762-I'M 1ST OF FRNDS HAVE NEW ELCTRNC EQUIP": "Disagre A Lot-I'M 1ST OF FRNDS HAVE NEW ELCTRNC EQUIP"})

#Let's go ahead and create an extra column to tally up the sum values for each row
txt1.assign(Sum=tx.select_dtypes(['number']).sum(1) - tx["v1-ID"])
v1-ID	Agree A Lot-I'M 1ST OF FRNDS HAVE NEW ELCTRNC EQUIP	Agree A Little-I'M 1ST OF FRNDS HAVE NEW ELCTRNC EQUIP	Neither Agree Or Disagree-I'M 1ST OF FRNDS HAVE NEW ELCTRNC EQUIP	Disagree A Little-I'M 1ST OF FRNDS HAVE NEW ELCTRNC EQUIP	Disagre A Lot-I'M 1ST OF FRNDS HAVE NEW ELCTRNC EQUIP	Sum
0	1985951	0.0	0.0	1.0	0.0	0.0	1.0
1	1985952	0.0	0.0	1.0	0.0	0.0	1.0
2	1985954	0.0	0.0	0.0	0.0	1.0	1.0
3	1985955	0.0	0.0	0.0	0.0	1.0	1.0
4	1985960	0.0	0.0	0.0	0.0	1.0	1.0
5	1985961	0.0	0.0	0.0	0.0	0.0	0.0
6	1985963	0.0	0.0	0.0	0.0	1.0	1.0
7	1985964	0.0	0.0	0.0	1.0	0.0	1.0
8	1985966	0.0	0.0	0.0	1.0	0.0	1.0
9	1985967	0.0	0.0	0.0	0.0	1.0	1.0
10	1985969	0.0	0.0	0.0	0.0	1.0	1.0
11	1985970	0.0	1.0	0.0	0.0	0.0	1.0
12	1985977	0.0	0.0	0.0	0.0	1.0	1.0
13	1985979	0.0	0.0	0.0	0.0	0.0	0.0
14	1985980	0.0	0.0	0.0	0.0	1.0	1.0
15	1985981	0.0	0.0	0.0	0.0	0.0	0.0
16	1985986	0.0	0.0	0.0	1.0	0.0	1.0
17	1985987	0.0	0.0	0.0	0.0	1.0	1.0
18	1985989	0.0	0.0	0.0	1.0	0.0	1.0
19	1985990	0.0	0.0	1.0	0.0	0.0	1.0
20	1985992	0.0	0.0	0.0	0.0	0.0	0.0
21	1985994	0.0	0.0	1.0	0.0	0.0	1.0
22	1985995	0.0	1.0	0.0	0.0	0.0	1.0
23	1985997	0.0	0.0	0.0	1.0	0.0	1.0
24	1985998	0.0	0.0	0.0	0.0	1.0	1.0
25	1986022	0.0	0.0	0.0	0.0	1.0	1.0
26	1986023	0.0	0.0	1.0	0.0	0.0	1.0
27	1986027	0.0	0.0	0.0	0.0	1.0	1.0
28	1986030	0.0	0.0	0.0	0.0	1.0	1.0
29	1986031	0.0	0.0	0.0	0.0	1.0	1.0
...	...	...	...	...	...	...	...
970	1990174	0.0	0.0	1.0	0.0	0.0	1.0
971	1990187	0.0	0.0	1.0	0.0	0.0	1.0
972	1990188	0.0	0.0	1.0	0.0	0.0	1.0
973	1990210	0.0	0.0	0.0	0.0	1.0	1.0
974	1990211	0.0	0.0	0.0	0.0	1.0	1.0
975	1990213	0.0	0.0	0.0	1.0	0.0	1.0
976	1990214	0.0	0.0	0.0	1.0	0.0	1.0
977	1990234	0.0	0.0	0.0	0.0	1.0	1.0
978	1990235	0.0	0.0	0.0	0.0	1.0	1.0
979	1990237	0.0	1.0	0.0	0.0	0.0	1.0
980	1990238	0.0	0.0	0.0	0.0	1.0	1.0
981	1990239	0.0	0.0	0.0	0.0	1.0	1.0
982	1990240	0.0	0.0	0.0	0.0	1.0	1.0
983	1990245	0.0	0.0	0.0	0.0	1.0	1.0
984	1990246	0.0	0.0	0.0	0.0	1.0	1.0
985	1990253	0.0	0.0	0.0	1.0	0.0	1.0
986	1990258	0.0	0.0	0.0	1.0	0.0	1.0
987	1990259	0.0	0.0	0.0	0.0	1.0	1.0
988	1990269	0.0	0.0	1.0	0.0	0.0	1.0
989	1990275	0.0	0.0	0.0	0.0	1.0	1.0
990	1990276	0.0	0.0	1.0	0.0	0.0	1.0
991	1990282	0.0	0.0	0.0	0.0	1.0	1.0
992	1990308	0.0	0.0	0.0	1.0	0.0	1.0
993	1990310	0.0	0.0	1.0	0.0	0.0	1.0
994	1990311	0.0	0.0	1.0	0.0	0.0	1.0
995	1990318	0.0	0.0	0.0	1.0	0.0	1.0
996	1990319	0.0	0.0	0.0	1.0	0.0	1.0
997	1990339	0.0	1.0	0.0	0.0	0.0	1.0
998	1990341	0.0	0.0	1.0	0.0	0.0	1.0
999	1990342	0.0	0.0	0.0	1.0	0.0	1.0
1000 rows × 7 columns

#As we may see, respondents were very attentive: there's only 48 instances of missed response entries,
#which represents 4.7% of all data. 
#Since it's such a negligible amount we could choose an option of dropping them as well.

#If needed, we could create an excel output for this dataframe.
tx.to_excel('tmp.xlsx')
#Let's select the next very entries for the Q#4. This will be attitudes related to traveling.
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 1000 entries, 0 to 999
Data columns (total 6 columns):
v1-ID                                                                1000 non-null int64
Agree A Lot-I'M 1ST OF FRNDS HAVE NEW ELCTRNC EQUIP                  1000 non-null float64
Agree A Little-I'M 1ST OF FRNDS HAVE NEW ELCTRNC EQUIP               1000 non-null float64
Neither Agree Or Disagree-I'M 1ST OF FRNDS HAVE NEW ELCTRNC EQUIP    1000 non-null float64
Disagree A Little-I'M 1ST OF FRNDS HAVE NEW ELCTRNC EQUIP            1000 non-null float64
Disagre A Lot-I'M 1ST OF FRNDS HAVE NEW ELCTRNC EQUIP                1000 non-null float64
dtypes: float64(5), int64(1)
memory usage: 46.9 KB
#Step1
import pandas as pd
import numpy as np
d = pd.read_excel("/home/PLN9630/FA12_Layout.xlsx")
d_short = f.iloc[6795:6851]
d_short["ID-Name"].unique()

#Step2
import pandas as pd

f = pd.read_excel("/home/PLN9630/FA12_Layout.xlsx")

f["Name"] = f["ID-Name"].rename()

widths = f.col_len.tolist()

names = f.Name.tolist()
#We will start with all 7 possible answers:
use_cols = [u"v1-ID",
            u'v6796-PREFER TRAVEL THE US OPPOSED TO FOREIGN', 
            u'v6804-PREFER TRAVEL THE US OPPOSED TO FOREIGN',
            u'v6812-PREFER TRAVEL THE US OPPOSED TO FOREIGN',
            u'v6820-PREFER TRAVEL THE US OPPOSED TO FOREIGN',
            u'v6828-PREFER TRAVEL THE US OPPOSED TO FOREIGN', 
            u'v6836-PREFER TRAVEL THE US OPPOSED TO FOREIGN',
            u'v6844-PREFER TRAVEL THE US OPPOSED TO FOREIGN',
           ]
tx = pd.read_fwf("/home/PLN9630/first1k.txt", widths=widths, names=names, usecols = use_cols)

tx[np.isnan(tx)] = 0
tx.head(10)
v1-ID	v6796-PREFER TRAVEL THE US OPPOSED TO FOREIGN	v6804-PREFER TRAVEL THE US OPPOSED TO FOREIGN	v6812-PREFER TRAVEL THE US OPPOSED TO FOREIGN	v6820-PREFER TRAVEL THE US OPPOSED TO FOREIGN	v6828-PREFER TRAVEL THE US OPPOSED TO FOREIGN	v6836-PREFER TRAVEL THE US OPPOSED TO FOREIGN	v6844-PREFER TRAVEL THE US OPPOSED TO FOREIGN
0	1985951	0.0	0.0	0.0	1.0	0.0	0.0	0.0
1	1985952	0.0	0.0	0.0	1.0	0.0	0.0	0.0
2	1985954	1.0	0.0	1.0	0.0	0.0	0.0	0.0
3	1985955	1.0	0.0	1.0	0.0	0.0	0.0	0.0
4	1985960	0.0	0.0	0.0	0.0	0.0	1.0	1.0
5	1985961	0.0	0.0	0.0	0.0	0.0	0.0	0.0
6	1985963	0.0	1.0	1.0	0.0	0.0	0.0	0.0
7	1985964	1.0	0.0	1.0	0.0	0.0	0.0	0.0
8	1985966	0.0	0.0	0.0	0.0	1.0	0.0	1.0
9	1985967	0.0	0.0	0.0	0.0	0.0	1.0	1.0
#Step3
print(tx.shape)
#(1000, 8)
#We have a thousand survey partcipants, which means we would expect to see each row to be sum up as one. If it's not equal to 1, 
#it's either was omitted or more than one option has been selected by a respondent.

#Let's rename the column titles to make it easier to understand the table.
txt = tx.rename(columns={u'v6796-PREFER TRAVEL THE US OPPOSED TO FOREIGN' : 'Agree A Lot-PREFER TRAVEL THE US OPPOSED TO FOREIGN', 
            u'v6804-PREFER TRAVEL THE US OPPOSED TO FOREIGN': 'Agree A Little-PREFER TRAVEL THE US OPPOSED TO FOREIGN',
            u'v6812-PREFER TRAVEL THE US OPPOSED TO FOREIGN': 'Any Agree-PREFER TRAVEL THE US OPPOSED TO FOREIGN',
            u'v6820-PREFER TRAVEL THE US OPPOSED TO FOREIGN': 'Neither Agree or Disagree-PREFER TRAVEL THE US OPPOSED TO FOREIGN',
            u'v6828-PREFER TRAVEL THE US OPPOSED TO FOREIGN': 'Disagree A Little-PREFER TRAVEL THE US OPPOSED TO FOREIGN', 
            u'v6836-PREFER TRAVEL THE US OPPOSED TO FOREIGN': 'Disagree A Lot-PREFER TRAVEL THE US OPPOSED TO FOREIGN',
            u'v6844-PREFER TRAVEL THE US OPPOSED TO FOREIGN': 'Any Disagree-PREFER TRAVEL THE US OPPOSED TO FOREIGN'})

#As we may see, keeping such columns as Any Agree or Any Disagree creates a duplication and possible further confusion. 
#One of potential possibilities would be to drop them alltogether and leave the rest. It would be worth considering keeping
#those variables in case when we would observe missed responses. They could have been potentially used a replacement.

#Let's go ahead and create an extra column to tally up the sum values for each row
txt.assign(Sum=tx.select_dtypes(['number']).sum(1) - tx["v1-ID"])
#As we may see, respondents were very attentive: there are no duplicated answers.
(1000, 8)
v1-ID	Agree A Lot-PREFER TRAVEL THE US OPPOSED TO FOREIGN	Agree A Little-PREFER TRAVEL THE US OPPOSED TO FOREIGN	Any Agree-PREFER TRAVEL THE US OPPOSED TO FOREIGN	Neither Agree or Disagree-PREFER TRAVEL THE US OPPOSED TO FOREIGN	Disagree A Little-PREFER TRAVEL THE US OPPOSED TO FOREIGN	Disagree A Lot-PREFER TRAVEL THE US OPPOSED TO FOREIGN	Any Disagree-PREFER TRAVEL THE US OPPOSED TO FOREIGN	Sum
0	1985951	0.0	0.0	0.0	1.0	0.0	0.0	0.0	1.0
1	1985952	0.0	0.0	0.0	1.0	0.0	0.0	0.0	1.0
2	1985954	1.0	0.0	1.0	0.0	0.0	0.0	0.0	2.0
3	1985955	1.0	0.0	1.0	0.0	0.0	0.0	0.0	2.0
4	1985960	0.0	0.0	0.0	0.0	0.0	1.0	1.0	2.0
5	1985961	0.0	0.0	0.0	0.0	0.0	0.0	0.0	0.0
6	1985963	0.0	1.0	1.0	0.0	0.0	0.0	0.0	2.0
7	1985964	1.0	0.0	1.0	0.0	0.0	0.0	0.0	2.0
8	1985966	0.0	0.0	0.0	0.0	1.0	0.0	1.0	2.0
9	1985967	0.0	0.0	0.0	0.0	0.0	1.0	1.0	2.0
10	1985969	1.0	0.0	1.0	0.0	0.0	0.0	0.0	2.0
11	1985970	0.0	1.0	1.0	0.0	0.0	0.0	0.0	2.0
12	1985977	1.0	0.0	1.0	0.0	0.0	0.0	0.0	2.0
13	1985979	0.0	0.0	0.0	0.0	0.0	0.0	0.0	0.0
14	1985980	0.0	0.0	0.0	0.0	1.0	0.0	1.0	2.0
15	1985981	0.0	0.0	0.0	0.0	0.0	1.0	1.0	2.0
16	1985986	0.0	0.0	0.0	1.0	0.0	0.0	0.0	1.0
17	1985987	0.0	0.0	0.0	1.0	0.0	0.0	0.0	1.0
18	1985989	1.0	0.0	1.0	0.0	0.0	0.0	0.0	2.0
19	1985990	1.0	0.0	1.0	0.0	0.0	0.0	0.0	2.0
20	1985992	0.0	0.0	0.0	1.0	0.0	0.0	0.0	1.0
21	1985994	0.0	0.0	0.0	1.0	0.0	0.0	0.0	1.0
22	1985995	1.0	0.0	1.0	0.0	0.0	0.0	0.0	2.0
23	1985997	0.0	0.0	0.0	0.0	0.0	0.0	0.0	0.0
24	1985998	1.0	0.0	1.0	0.0	0.0	0.0	0.0	2.0
25	1986022	0.0	0.0	0.0	0.0	0.0	0.0	0.0	0.0
26	1986023	0.0	0.0	0.0	1.0	0.0	0.0	0.0	1.0
27	1986027	1.0	0.0	1.0	0.0	0.0	0.0	0.0	2.0
28	1986030	0.0	0.0	0.0	0.0	0.0	1.0	1.0	2.0
29	1986031	1.0	0.0	1.0	0.0	0.0	0.0	0.0	2.0
...	...	...	...	...	...	...	...	...	...
970	1990174	0.0	0.0	0.0	0.0	1.0	0.0	1.0	2.0
971	1990187	0.0	0.0	0.0	1.0	0.0	0.0	0.0	1.0
972	1990188	0.0	0.0	0.0	1.0	0.0	0.0	0.0	1.0
973	1990210	0.0	0.0	0.0	0.0	0.0	1.0	1.0	2.0
974	1990211	0.0	0.0	0.0	0.0	0.0	1.0	1.0	2.0
975	1990213	1.0	0.0	1.0	0.0	0.0	0.0	0.0	2.0
976	1990214	1.0	0.0	1.0	0.0	0.0	0.0	0.0	2.0
977	1990234	1.0	0.0	1.0	0.0	0.0	0.0	0.0	2.0
978	1990235	1.0	0.0	1.0	0.0	0.0	0.0	0.0	2.0
979	1990237	0.0	0.0	0.0	0.0	1.0	0.0	1.0	2.0
980	1990238	0.0	0.0	0.0	1.0	0.0	0.0	0.0	1.0
981	1990239	1.0	0.0	1.0	0.0	0.0	0.0	0.0	2.0
982	1990240	1.0	0.0	1.0	0.0	0.0	0.0	0.0	2.0
983	1990245	0.0	0.0	0.0	0.0	1.0	0.0	1.0	2.0
984	1990246	0.0	0.0	0.0	1.0	0.0	0.0	0.0	1.0
985	1990253	0.0	0.0	0.0	1.0	0.0	0.0	0.0	1.0
986	1990258	0.0	1.0	1.0	0.0	0.0	0.0	0.0	2.0
987	1990259	0.0	0.0	0.0	1.0	0.0	0.0	0.0	1.0
988	1990269	0.0	0.0	0.0	0.0	0.0	0.0	0.0	0.0
989	1990275	0.0	0.0	0.0	0.0	0.0	1.0	1.0	2.0
990	1990276	0.0	0.0	0.0	1.0	0.0	0.0	0.0	1.0
991	1990282	1.0	0.0	1.0	0.0	0.0	0.0	0.0	2.0
992	1990308	0.0	1.0	1.0	0.0	0.0	0.0	0.0	2.0
993	1990310	0.0	0.0	0.0	0.0	0.0	0.0	0.0	0.0
994	1990311	0.0	0.0	0.0	0.0	1.0	0.0	1.0	2.0
995	1990318	0.0	1.0	1.0	0.0	0.0	0.0	0.0	2.0
996	1990319	0.0	1.0	1.0	0.0	0.0	0.0	0.0	2.0
997	1990339	0.0	0.0	0.0	0.0	1.0	0.0	1.0	2.0
998	1990341	0.0	0.0	0.0	1.0	0.0	0.0	0.0	1.0
999	1990342	0.0	1.0	1.0	0.0	0.0	0.0	0.0	2.0
1000 rows × 9 columns

#Repeat Step2
#Let's update the dataset by removing "Any Disagree" and "Any Agree" values
f = pd.read_excel("/home/PLN9630/FA12_Layout.xlsx")

f["Name"] = f["ID-Name"].rename()


widths = f.col_len.tolist()

names = f.Name.tolist()

use_cols = [u"v1-ID",
            u'v6796-PREFER TRAVEL THE US OPPOSED TO FOREIGN', 
            u'v6804-PREFER TRAVEL THE US OPPOSED TO FOREIGN',
            u'v6820-PREFER TRAVEL THE US OPPOSED TO FOREIGN',
            u'v6828-PREFER TRAVEL THE US OPPOSED TO FOREIGN', 
            u'v6836-PREFER TRAVEL THE US OPPOSED TO FOREIGN']
# we selcted 5 options for the answer : "Agree A Lot", Agree A Little", "Neither Agree or Disagree", "Disagree A Little", "Disagree A Lot"
# we selcted 5 options for the answer : "Agree A Lot", Agree A Little", "Neither Agree or Disagree", "Disagree A Little", "Disagree A Lot"
tx = pd.read_fwf("/home/PLN9630/first1k.txt", widths=widths, names=names, usecols = use_cols)

tx[np.isnan(tx)] = 0
tx.head(10)

#We have a thousand survey partcipants, which means we would expect to see each row to be sum up as one. If it's not equal to 1, 
#it's either was omitted or more than one option has been selected by a respondent.
txt1 = tx.rename(columns={u'v6796-PREFER TRAVEL THE US OPPOSED TO FOREIGN' : 'Agree A Lot-PREFER TRAVEL THE US OPPOSED TO FOREIGN', 
            u'v6804-PREFER TRAVEL THE US OPPOSED TO FOREIGN': 'Agree A Little-PREFER TRAVEL THE US OPPOSED TO FOREIGN',
            u'v6820-PREFER TRAVEL THE US OPPOSED TO FOREIGN': 'Neither Agree or Disagree-PREFER TRAVEL THE US OPPOSED TO FOREIGN',
            u'v6828-PREFER TRAVEL THE US OPPOSED TO FOREIGN': 'Disagree A Little-PREFER TRAVEL THE US OPPOSED TO FOREIGN', 
            u'v6836-PREFER TRAVEL THE US OPPOSED TO FOREIGN': 'Disagree A Lot-PREFER TRAVEL THE US OPPOSED TO FOREIGN'})

#Let's go ahead and create an extra column to tally up the sum values for each row
txt1.assign(Sum=tx.select_dtypes(['number']).sum(1) - tx["v1-ID"])
v1-ID	Agree A Lot-PREFER TRAVEL THE US OPPOSED TO FOREIGN	Agree A Little-PREFER TRAVEL THE US OPPOSED TO FOREIGN	Neither Agree or Disagree-PREFER TRAVEL THE US OPPOSED TO FOREIGN	Disagree A Little-PREFER TRAVEL THE US OPPOSED TO FOREIGN	Disagree A Lot-PREFER TRAVEL THE US OPPOSED TO FOREIGN	Sum
0	1985951	0.0	0.0	1.0	0.0	0.0	1.0
1	1985952	0.0	0.0	1.0	0.0	0.0	1.0
2	1985954	1.0	0.0	0.0	0.0	0.0	1.0
3	1985955	1.0	0.0	0.0	0.0	0.0	1.0
4	1985960	0.0	0.0	0.0	0.0	1.0	1.0
5	1985961	0.0	0.0	0.0	0.0	0.0	0.0
6	1985963	0.0	1.0	0.0	0.0	0.0	1.0
7	1985964	1.0	0.0	0.0	0.0	0.0	1.0
8	1985966	0.0	0.0	0.0	1.0	0.0	1.0
9	1985967	0.0	0.0	0.0	0.0	1.0	1.0
10	1985969	1.0	0.0	0.0	0.0	0.0	1.0
11	1985970	0.0	1.0	0.0	0.0	0.0	1.0
12	1985977	1.0	0.0	0.0	0.0	0.0	1.0
13	1985979	0.0	0.0	0.0	0.0	0.0	0.0
14	1985980	0.0	0.0	0.0	1.0	0.0	1.0
15	1985981	0.0	0.0	0.0	0.0	1.0	1.0
16	1985986	0.0	0.0	1.0	0.0	0.0	1.0
17	1985987	0.0	0.0	1.0	0.0	0.0	1.0
18	1985989	1.0	0.0	0.0	0.0	0.0	1.0
19	1985990	1.0	0.0	0.0	0.0	0.0	1.0
20	1985992	0.0	0.0	1.0	0.0	0.0	1.0
21	1985994	0.0	0.0	1.0	0.0	0.0	1.0
22	1985995	1.0	0.0	0.0	0.0	0.0	1.0
23	1985997	0.0	0.0	0.0	0.0	0.0	0.0
24	1985998	1.0	0.0	0.0	0.0	0.0	1.0
25	1986022	0.0	0.0	0.0	0.0	0.0	0.0
26	1986023	0.0	0.0	1.0	0.0	0.0	1.0
27	1986027	1.0	0.0	0.0	0.0	0.0	1.0
28	1986030	0.0	0.0	0.0	0.0	1.0	1.0
29	1986031	1.0	0.0	0.0	0.0	0.0	1.0
...	...	...	...	...	...	...	...
970	1990174	0.0	0.0	0.0	1.0	0.0	1.0
971	1990187	0.0	0.0	1.0	0.0	0.0	1.0
972	1990188	0.0	0.0	1.0	0.0	0.0	1.0
973	1990210	0.0	0.0	0.0	0.0	1.0	1.0
974	1990211	0.0	0.0	0.0	0.0	1.0	1.0
975	1990213	1.0	0.0	0.0	0.0	0.0	1.0
976	1990214	1.0	0.0	0.0	0.0	0.0	1.0
977	1990234	1.0	0.0	0.0	0.0	0.0	1.0
978	1990235	1.0	0.0	0.0	0.0	0.0	1.0
979	1990237	0.0	0.0	0.0	1.0	0.0	1.0
980	1990238	0.0	0.0	1.0	0.0	0.0	1.0
981	1990239	1.0	0.0	0.0	0.0	0.0	1.0
982	1990240	1.0	0.0	0.0	0.0	0.0	1.0
983	1990245	0.0	0.0	0.0	1.0	0.0	1.0
984	1990246	0.0	0.0	1.0	0.0	0.0	1.0
985	1990253	0.0	0.0	1.0	0.0	0.0	1.0
986	1990258	0.0	1.0	0.0	0.0	0.0	1.0
987	1990259	0.0	0.0	1.0	0.0	0.0	1.0
988	1990269	0.0	0.0	0.0	0.0	0.0	0.0
989	1990275	0.0	0.0	0.0	0.0	1.0	1.0
990	1990276	0.0	0.0	1.0	0.0	0.0	1.0
991	1990282	1.0	0.0	0.0	0.0	0.0	1.0
992	1990308	0.0	1.0	0.0	0.0	0.0	1.0
993	1990310	0.0	0.0	0.0	0.0	0.0	0.0
994	1990311	0.0	0.0	0.0	1.0	0.0	1.0
995	1990318	0.0	1.0	0.0	0.0	0.0	1.0
996	1990319	0.0	1.0	0.0	0.0	0.0	1.0
997	1990339	0.0	0.0	0.0	1.0	0.0	1.0
998	1990341	0.0	0.0	1.0	0.0	0.0	1.0
999	1990342	0.0	1.0	0.0	0.0	0.0	1.0
1000 rows × 7 columns

#As we may see, respondents were very attentive: there's only 79 instances of missed response entries,
#which is a little less than 8% of the data overall. 
#Since it's such a negligible amount we could choose an option of dropping them as well.

#If needed, we could create an excel output for this dataframe.
tx.to_excel('tmpd.xlsx')
