People always say that being active brings you happiness. There's been scientific research about the increase in seretonin levels and generally when you are active, you are in a healthier state which can also bring happiness. Some people grew up playing sports or some found interests in athletic activities later on in life, but it is evident that being active can bring happiness. On the flip side, life can be stressful- and when life is stressful, participating in activties that are considered as leisure would be more enjoyable and bring more happiness. After a long day at work or school or taking care of kids, sometimes happiness comes from relaxing rather than being active. In this project, I wanted to explore the difference between exercise and leisure being a source of happiness.

Based on the word frequency chart, it is obvious that leisure activities brought people more happiness than exercise. The difference is vast as there were 7458 happy moments based in leisure while only 1202 happy moments based in exercise. I wanted to look further into this to see what specifically brings people happiness in each of these categories. 


```python
#frequency of happy moments based in exercise or leisure

import pandas as pd
import matplotlib.pyplot as plt

cleaned_hm = pd.read_csv("HappyDB/happydb/data/cleaned_hm.csv")
unique_val = pd.DataFrame(cleaned_hm["predicted_category"].value_counts())
values = cleaned_hm['predicted_category'].value_counts().keys().tolist()
counts = cleaned_hm['predicted_category'].value_counts().tolist()

df = pd.DataFrame(index = values)
df['values'] = values
df['counts'] = counts
df = df.drop(['achievement', 'affection', 'bonding', 'enjoy_the_moment', 'nature'])
print(counts)

ax = df.plot(kind = 'barh')
ax.legend_ = None
ax.set_xlabel("Word Frequency", labelpad = 20, size = 12)
ax.set_ylabel("Word Category", labelpad = 20, size = 12)
```

    [34168, 33993, 11144, 10727, 7458, 1843, 1202]





    Text(0, 0.5, 'Word Category')




    
![png](output_2_2.png)
    


Based on the word cloud, its evident that activities relating to watching something (movie, tv show, etc.) were the primary sources of happy moments categorized as leisure. Taking trips and vacations also seem to be another form of lesiure that brings people happiness.


```python
#common words used in happy moments based on leisure

from wordcloud import WordCloud, STOPWORDS
hmid_leisure = cleaned_hm[cleaned_hm['predicted_category'] ==  'leisure']['hmid']
senselabel = pd.read_csv("HappyDB/happydb/data/senselabel.csv")
SL_leisure = senselabel[senselabel['hmid'].isin(hmid_leisure)]
SL_leisure = SL_leisure.sort_values('hmid')
leisure_verb_noun = SL_leisure[(SL_leisure.POS != 'PUNCT') & (SL_leisure.POS != 'PRON') & (SL_leisure.POS != 'ADJ') & (SL_leisure.POS != 'ADV')
          & (SL_leisure.POS != 'NUM') & (SL_leisure.POS != 'DET') & (SL_leisure.POS != 'PRT') & (SL_leisure.POS != 'ADP') 
                               & (SL_leisure.POS != 'CONJ') & (SL_leisure.POS != 'X') & (SL_leisure.lowercaseLemma != 'i') & (SL_leisure.lowercaseLemma != 'be')
                              & (SL_leisure.lowercaseLemma != 'go') & (SL_leisure.lowercaseLemma != 'have') & (SL_leisure.lowercaseLemma != 'get') 
                               & (SL_leisure.lowercaseLemma != 'went') & (SL_leisure.lowercaseLemma != 'take') & (SL_leisure.lowercaseLemma != 'make') & (SL_leisure.lowercaseLemma != 'watch')
                              & (SL_leisure.lowercaseLemma != 'time') & (SL_leisure.lowercaseLemma != 'day') & (SL_leisure.lowercaseLemma != 'night') & (SL_leisure.lowercaseLemma != 'see')
                              & (SL_leisure.lowercaseLemma != 'saw') & (SL_leisure.lowercaseLemma != 'come') & (SL_leisure.lowercaseLemma != 'enjoy') & (SL_leisure.lowercaseLemma != 'watching')
                              & (SL_leisure.lowercaseLemma != 'today') & (SL_leisure.lowercaseLemma != 'week') & (SL_leisure.lowercaseLemma != 'hour') & (SL_leisure.lowercaseLemma != 'find')
                              & (SL_leisure.lowercaseLemma != 'want') & (SL_leisure.lowercaseLemma != 'do') & (SL_leisure.lowercaseLemma != 'lot') & (SL_leisure.lowercaseLemma != 'seeing') &
                              (SL_leisure.lowercaseLemma != 'way') & (SL_leisure.lowercaseLemma != 'switch') & (SL_leisure.lowercaseLemma != 'put') & (SL_leisure.lowercaseLemma != 're') 
                               & (SL_leisure.lowercaseLemma != 'try') & (SL_leisure.lowercaseLemma != 'felt') & (SL_leisure.lowercaseLemma != 'feel') & (SL_leisure.lowercaseLemma != 'begin')
                              & (SL_leisure.lowercaseLemma != 'will')& (SL_leisure.lowercaseLemma != 'pick')& (SL_leisure.lowercaseLemma != 'use')& (SL_leisure.lowercaseLemma != 'need')
                              & (SL_leisure.lowercaseLemma != 'got')& (SL_leisure.lowercaseLemma != 'realize') & (SL_leisure.lowercaseLemma != 'wait') & (SL_leisure.lowercaseLemma != 'part') & (SL_leisure.lowercaseLemma != 'watched')
                              & (SL_leisure.lowercaseLemma != 'getting')& (SL_leisure.lowercaseLemma != 're')& (SL_leisure.lowercaseLemma != 'try') & (SL_leisure.lowercaseLemma != 's')& (SL_leisure.lowercaseLemma != 'm')
                              & (SL_leisure.lowercaseLemma != 'decide')& (SL_leisure.lowercaseLemma != 'level') & (SL_leisure.lowercaseLemma != 's') & (SL_leisure.lowercaseLemma != 've') & (SL_leisure.lowercaseLemma != 'yesterday')
                              & (SL_leisure.lowercaseLemma != 'started') & (SL_leisure.lowercaseLemma != 'starting') & (SL_leisure.lowercaseLemma != 'start') & (SL_leisure.lowercaseLemma != 'play')  ]
print(leisure_verb_noun["lowercaseLemma"].value_counts().head(10))
text = " ".join(str(lowercaseLemma) for lowercaseLemma in leisure_verb_noun.lowercaseLemma)
wordcloud = WordCloud(background_color ='white', width = 400, height = 400, stopwords = list(STOPWORDS)).generate(text)
plt.figure(facecolor = None)
plt.imshow(wordcloud)
plt.axis("off")
plt.tight_layout(pad = 0)
plt.show()
```

    game        1121
    movie        831
    show         705
    video        469
    tv           394
    read         350
    episode      321
    book         314
    shopping     303
    season       295
    Name: lowercaseLemma, dtype: int64



    
![png](output_4_1.png)
    


For happy moments based in exercise, it seems like people enjoy going to the gym, running, walking and doing yoga.


```python
#common words used in happy moments based on exercise
from wordcloud import WordCloud, STOPWORDS

exercise_list = pd.read_csv("HappyDB/happydb/data/topic_dict/exercise-dict.csv")
hmid_exercise = cleaned_hm[cleaned_hm['predicted_category'] ==  'exercise']['hmid']
SL_exercise = senselabel[senselabel['hmid'].isin(hmid_exercise)]
SL_exercise = SL_exercise.sort_values('hmid')
exercise_keywords = SL_exercise[SL_exercise['lowercaseLemma'].isin(exercise_list['Words'])]

print(exercise_keywords["lowercaseLemma"].value_counts().head(10))
text = " ".join(str(lowercaseLemma) for lowercaseLemma in exercise_keywords.lowercaseLemma)
wordcloud = WordCloud(background_color ='white', width = 400, height = 400, stopwords = list(STOPWORDS)).generate(text)
plt.figure(facecolor = None)
plt.imshow(wordcloud)
plt.axis("off")
plt.tight_layout(pad = 0)
plt.show()
```

    gym          416
    run          289
    workout      259
    exercise     161
    yoga         103
    walk          57
    bike          45
    jog           31
    running       21
    treadmill     18
    Name: lowercaseLemma, dtype: int64



    
![png](output_6_1.png)
    


I next wanted to explore if there was a difference between genders in what brings them more happines between exercise and leisure. As a percentage of the female responses and male responses, respectively, it is still evident that leisure is the core of more happy moments than exercise. Interestingly enough, a greater percentage of the male responses (> 8%) were categorized as leisure compared to women (>6%). In regard to happy moments based in exercise, it's evident that a greater percentage of the male happy moments (> 1%) were based in exercise compared to women, which was < 1%. These results weren't particularly surprising as regardless of gender, people usually find joy in relaxing rather than doing something that requires a lot of effort.


```python
#Does the preference between exercise or leisure as a source of happiness vary between genders?

import pandas as pd
import matplotlib.pyplot as plt
import streamlit as st
import numpy as np

cleaned_hm = pd.read_csv("HappyDB/happydb/data/cleaned_hm.csv")
demographic = pd.read_csv("HappyDB/happydb/data/demographic.csv")

wid_female = demographic[demographic['gender'] ==  'f']['wid']
female = cleaned_hm[cleaned_hm['wid'].isin(wid_female)]
female = female.sort_values('wid')
f_unique_val = pd.DataFrame(female["predicted_category"].value_counts())
f_values = female['predicted_category'].value_counts().keys().tolist()
f_counts = female['predicted_category'].value_counts().tolist()


df_female = pd.DataFrame(index = f_values)
df_female['values'] = f_values
df_female['counts'] = f_counts
df_female = df_female.drop(['achievement', 'affection', 'bonding', 'enjoy_the_moment', 'nature'])
df_female = pd.DataFrame((df_female['counts'] / sum(f_counts))*100)

wid_male = demographic[demographic['gender'] ==  'm']['wid']
male = cleaned_hm[cleaned_hm['wid'].isin(wid_male)]
male = male.sort_values('wid')
m_unique_val = pd.DataFrame(male["predicted_category"].value_counts())
m_values = male['predicted_category'].value_counts().keys().tolist()
m_counts = male['predicted_category'].value_counts().tolist()

df_male = pd.DataFrame(index = m_values)
df_male['values'] = m_values
df_male['counts'] = m_counts
df_male = df_male.drop(['achievement', 'affection', 'bonding', 'enjoy_the_moment', 'nature'])
df_male = pd.DataFrame((df_male['counts'] / sum(m_counts))*100)

female_le_ex = pd.DataFrame([df_female.iloc[0], df_female.iloc[1]])
male_le_ex = pd.DataFrame([df_male.iloc[0], df_male.iloc[1]])
index = ['female', 'male']

#plots
ax_f = female_le_ex.plot(kind = 'barh')
ax_f.legend_ = None
ax_f.set_xlabel("% of Female Happy Moments", labelpad=20, size=12)
ax_f.set_ylabel("Word Category", labelpad=20, size=12)

ax_m = male_le_ex.plot(kind = 'barh')
ax_m.legend_ = None
ax_m.set_xlabel("% of Male Happy Moments", labelpad=20, size=12)
ax_m.set_ylabel("Word Category", labelpad=20, size=12)

```




    Text(0, 0.5, 'Word Category')




    
![png](output_8_1.png)
    



    
![png](output_8_2.png)
    


I next thought it would be interesting to see the difference in happy moments based in leisure vs exercise between parents and non-parents. I thought about this because I know that parents tend to focus more of their energy on taking care of their kids, so they may have more moments of happiness when they're relaxing rather than doing something else that requires a lot of effort. Instead, these results really surprised me at first. It's evident that of the responses by non-parents, a greater percentage experienced leisure as a source of happiness (> 8%) in comparison to parents (> 5%). I hypothesize that many happy moments experienced by parents were probably categorized as affection, bonding, and/or enjoy the moment since they typically prioritize their family. 

In regard to the happy moments based in exercise, the results did not surprise me as a fewer percentage of the total repsonses by parents reflected exercise as a source of happiness (< 1%) in comparison to non-parents (> 1%). This makes sense as non-parents may have more time on their hands to exercise than parents. 


```python
#Does the preference between exercise or leisure as a source of happiness vary between parents and non-parents?

import pandas as pd
import matplotlib.pyplot as plt
import streamlit as st
import numpy as np

cleaned_hm = pd.read_csv("HappyDB/happydb/data/cleaned_hm.csv")
demographic = pd.read_csv("HappyDB/happydb/data/demographic.csv")

wid_parent = demographic[demographic['parenthood'] ==  'y']['wid']
parent = cleaned_hm[cleaned_hm['wid'].isin(wid_parent)]
parent = parent.sort_values('wid')
p_unique_val = pd.DataFrame(parent["predicted_category"].value_counts())
p_values = parent['predicted_category'].value_counts().keys().tolist()
p_counts = parent['predicted_category'].value_counts().tolist()


df_parent = pd.DataFrame(index = p_values)
df_parent['values'] = p_values
df_parent['counts'] = p_counts
df_parent = df_parent.drop(['achievement', 'affection', 'bonding', 'enjoy_the_moment', 'nature'])
df_parent = pd.DataFrame((df_parent['counts'] / sum(p_counts))*100)

wid_nonparent = demographic[demographic['parenthood'] ==  'n']['wid']
nonparent = cleaned_hm[cleaned_hm['wid'].isin(wid_nonparent)]
nonparent = nonparent.sort_values('wid')
np_unique_val = pd.DataFrame(nonparent["predicted_category"].value_counts())
np_values = nonparent['predicted_category'].value_counts().keys().tolist()
np_counts = nonparent['predicted_category'].value_counts().tolist()

df_nonparent = pd.DataFrame(index = np_values)
df_nonparent['values'] = np_values
df_nonparent['counts'] = np_counts
df_nonparent = df_nonparent.drop(['achievement', 'affection', 'bonding', 'enjoy_the_moment', 'nature'])
df_nonparent = pd.DataFrame((df_nonparent['counts'] / sum(np_counts))*100)

parent_le_ex = pd.DataFrame([df_parent.iloc[0], df_parent.iloc[1]])
nonparent_le_ex = pd.DataFrame([df_nonparent.iloc[0], df_nonparent.iloc[1]])
index = ['parents', 'non-parents']

#plots
ax_p = parent_le_ex.plot(kind = 'barh')
ax_p.legend_ = None
ax_p.set_xlabel("% of Parent Happy Moments", labelpad=20, size=12)
ax_p.set_ylabel("Word Category", labelpad=20, size=12)

ax_np = nonparent_le_ex.plot(kind = 'barh')
ax_np.legend_ = None
ax_np.set_xlabel("% of Non-Parent Happy Moments", labelpad=20, size=12)
ax_np.set_ylabel("Word Category", labelpad=20, size=12)
```




    Text(0, 0.5, 'Word Category')




    
![png](output_10_1.png)
    



    
![png](output_10_2.png)
    


I finally wanted to observe the dichotomy of happy moments based in exercise vs leisure across age groups. I split up the particpants into three categories: 0-30, 31-60, and 61-100. The results shown did not surprise me. The percentage of happy moments based in exercise decreases as the age groups increased which makes sense as it gets harder and more cumbersome to exert physical effort as you age. 


The percentage of happy moments based in leisure stayed pretty consistent with a slight decrease from > 8% to > 6% as the age group shifted from 0-30 to 31-60. This increased to > 7% for the oldest age group. This pattern makes sense as younger people tend to want to relax, watch tv, go on trips, etc. but between the ages of 31-60, a lot of people are at the height of their careers and are also building families. Their happiness would then be sourced from other things such as their family or sense of achievement. This is consistent with the results from the parent vs non-parent comparison where parents find happiness in other things other than leisure, more so than non-parents. Then finally, as people enter into the elderly stage of their life, they tend to want to settle down and relax, which explains the uptick in leisure as a source of happiness across responses for people between ages 61-100. 


```python
#Does the preference between exercise or leisure as a source of happiness vary across age groups?

import pandas as pd
import matplotlib.pyplot as plt
import streamlit as st
import numpy as np

cleaned_hm = pd.read_csv("HappyDB/happydb/data/cleaned_hm.csv")
demographic = pd.read_csv("HappyDB/happydb/data/demographic.csv")


demographic['age'] = pd.to_numeric(demographic['age'], errors = 'coerce')
demographic = demographic[demographic['age'].notna()].astype({'age': int})

#age 10-30
wid_zero_30 = demographic[(demographic['age'] >=  0) & (demographic['age'] <= 30)]['wid']
zero_30 = cleaned_hm[cleaned_hm['wid'].isin(wid_zero_30)]
zero_30 = zero_30.sort_values('wid')
zero_30_unique_val = pd.DataFrame(zero_30["predicted_category"].value_counts())
zero_30_values = zero_30['predicted_category'].value_counts().keys().tolist()
zero_30_counts = zero_30['predicted_category'].value_counts().tolist()

df_zero_30 = pd.DataFrame(index = zero_30_values)
df_zero_30['values'] = zero_30_values
df_zero_30['counts'] = zero_30_counts
df_zero_30 = df_zero_30.drop(['achievement', 'affection', 'bonding', 'enjoy_the_moment', 'nature'])
df_zero_30 = pd.DataFrame((df_zero_30['counts'] / sum(zero_30_counts))*100)


#age 31-60
wid_thirtyone_60 = demographic[(demographic['age'] >=  31) & (demographic['age'] <= 60)]['wid']
thirtyone_60= cleaned_hm[cleaned_hm['wid'].isin(wid_thirtyone_60)]
thirtyone_60 = thirtyone_60.sort_values('wid')
thirtyone_60_unique_val = pd.DataFrame(thirtyone_60["predicted_category"].value_counts())
thirtyone_60_values = thirtyone_60['predicted_category'].value_counts().keys().tolist()
thirtyone_60_counts = thirtyone_60['predicted_category'].value_counts().tolist()

df_thirtyone_60 = pd.DataFrame(index = thirtyone_60_values)
df_thirtyone_60['values'] = thirtyone_60_values
df_thirtyone_60['counts'] = thirtyone_60_counts
df_thirtyone_60 = df_thirtyone_60.drop(['achievement', 'affection', 'bonding', 'enjoy_the_moment', 'nature'])
df_thirtyone_60 = pd.DataFrame((df_thirtyone_60['counts'] / sum(thirtyone_60_counts))*100)


#age 61-100
wid_sixtyone_100 = demographic[(demographic['age'] >=  61) & (demographic['age'] <= 100)]['wid']
sixtyone_100 = cleaned_hm[cleaned_hm['wid'].isin(wid_sixtyone_100)]
sixtyone_100 = sixtyone_100.sort_values('wid')
sixtyone_100_unique_val = pd.DataFrame(sixtyone_100["predicted_category"].value_counts())
sixtyone_100_values = sixtyone_100['predicted_category'].value_counts().keys().tolist()
sixtyone_100_counts = sixtyone_100['predicted_category'].value_counts().tolist()


df_sixtyone_100 = pd.DataFrame(index = sixtyone_100_values)
df_sixtyone_100['values'] = sixtyone_100_values
df_sixtyone_100['counts'] = sixtyone_100_counts
df_sixtyone_100 = df_sixtyone_100.drop(['achievement', 'affection', 'bonding', 'enjoy_the_moment', 'nature'])
df_sixtyone_100 = pd.DataFrame((df_sixtyone_100['counts'] / sum(sixtyone_100_counts))*100)

#plots
zero_30_le_ex = pd.DataFrame([df_zero_30.iloc[0], df_zero_30.iloc[1]])
thirtyone_60_le_ex = pd.DataFrame([df_thirtyone_60.iloc[0], df_thirtyone_60.iloc[1]])
sixtyone_100_le_ex = pd.DataFrame([df_sixtyone_100.iloc[0], df_sixtyone_100.iloc[1]])
index = ['0-30', '31-60', '61-90']
ax_30 = zero_30_le_ex.plot(kind = 'barh')
ax_30.legend_ = None
ax_30.set_xlabel("% of Happy Moments of People Aged 0-30", labelpad=20, size=12)
ax_30.set_ylabel("Word Category", labelpad=20, size=12)

ax_60 = thirtyone_60_le_ex.plot(kind = 'barh')
ax_60.legend_ = None
ax_60.set_xlabel("% of Happy Moments of People Aged 31-60", labelpad=20, size=12)
ax_60.set_ylabel("Word Category", labelpad=20, size=12)

ax_100 = sixtyone_100_le_ex.plot(kind = 'barh')
ax_100.legend_ = None
ax_100.set_xlabel("% of Happy Moments of People Aged 61-100", labelpad=20, size=12)
ax_100.set_ylabel("Word Category", labelpad=20, size=12)
```




    Text(0, 0.5, 'Word Category')




    
![png](output_12_1.png)
    



    
![png](output_12_2.png)
    



    
![png](output_12_3.png)
    


In conclusion, it is consistent across all the data that people find more happiness in leisure than exercise. While exercise may make you feel better physically, genuine happiness comes from simpler, more relaxing moments such as watching tv, vacationing, and resting. This trend is seen across genders, parenthood status, and age groups.

Another thing that is evident is that people who are likely experiencing signifcant life changes will prioritize other things or other people other than themselves, so their happiness may not be based in leisure as much as other people. People who are building a family, building their career, and generally experiencing the events that come with being middle-aged have less happy moments based in leisure than people who aren't experiencing these things.

These results are telling as it shows me that taking care of myself mentally and spiritually can allow me to have more happy moments in comparison to focusing too heavily on my physical self. Being at peace is essential to happiness.


```python

```
