import requests
import json
from bs4 import BeautifulSoup
import pandas as pd
import re

dataStr = """ro: 0
kwop: 7
keyword: python
expansionType: area,spec,com,job,wf,wktm
order: 15
asc: 0
page: 10
mode: s
jobsource: 2018indexpoc
langFlag: 0
langStatus: 0
recommendJob: 1
hotJob: 1"""
data = {r.split(": ")[0]: r.split(": ")[1] for r in dataStr.split("\n")}

company_list = []
jobname_list = []
job_content_list = []
article_url_list = []
python_list = []
js_list = []
java_list = []
mysql_list = []
mongodb_list = []
sql_list = []
nosql_list = []

# 抓不同頁
for i in range(1,3):
    url = f"https://www.104.com.tw/jobs/search/?ro=0&keyword=python&expansionType=area%2Cspec%2Ccom%2Cjob%2Cwf%2Cwktm&order=15&asc=0&page={i}&mode=s&jobsource=2018indexpoc&langFlag=0&langStatus=0&recommendJob=1&hotJob=1"
    headers = {"User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/106.0.0.0 Safari/537.36"}
    res = requests.post(url, headers=headers, data=data)
    soup = BeautifulSoup(res.text, 'html.parser')
    # 公司名稱
    company = soup.select('ul[class="b-list-inline b-clearfix"]')
    for each_company in company:
        result = each_company.a["title"].split("：")[1].split("\n")[0]
        company_list.append(result)
    # 職稱
    article_url = soup.select('a[class="js-job-link"]')
    for each_jobname in article_url:
        result = each_jobname.text
        jobname_list.append(result)
    # 工作內容
    job_content = soup.select('p[class="job-list-item__info b-clearfix b-content"]')
    for each_job_content in job_content:
        result = each_job_content.text
        job_content_list.append(result)
    # 各職缺網址
    for each_url in article_url:
        result = 'https:' + each_url["href"]
        article_url_list.append(result)
    # 找關鍵字python、javascript、java、mysql......
    for each_job_content in job_content:
        if re.search("python",each_job_content.text,re.IGNORECASE):
            python_list.append("1")
        else:
            python_list.append("0")
    for each_job_content in job_content:
        if re.search("javascript",each_job_content.text,re.IGNORECASE):
            js_list.append("1")
        else:
            js_list.append("0")
    for each_job_content in job_content:
        if re.search("java",each_job_content.text,re.IGNORECASE):
            java_list.append("1")
        else:
            java_list.append("0")
    for each_job_content in job_content:
        if re.search("mysql",each_job_content.text,re.IGNORECASE):
            mysql_list.append("1")
        else:
            mysql_list.append("0")
    for each_job_content in job_content:
        if re.search("mongodb",each_job_content.text,re.IGNORECASE):
            mongodb_list.append("1")
        else:
            mongodb_list.append("0")
    for each_job_content in job_content:
        if re.search("sql",each_job_content.text,re.IGNORECASE):
            sql_list.append("1")
        else:
            sql_list.append("0")
    for each_job_content in job_content:
        if re.search("nosql",each_job_content.text,re.IGNORECASE):
            nosql_list.append("1")
        else:
            nosql_list.append("0")
    
df = pd.DataFrame(columns=["company_name"],data=company_list)
df["jobname"] = jobname_list
df["job_content"] = job_content_list
df["url"] = article_url_list
df["Python"] = python_list
df["JavaScript"] = js_list
df["Java"] = java_list
df["MySQL"] = mysql_list
df["mongodb"] = mongodb_list
df["sql"] = sql_list
df["nosql"] = nosql_list

df.to_excel("./104job.xlsx", index=False, encoding="utf-8-sig")

# 可以把df秀出來
