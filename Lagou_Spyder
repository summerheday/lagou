import requests  
import math    
import time  
import pymysql

db = pymysql.connect(host='localhost', user='root', password='hexin123', port=3306, db='lagou')
cursor = db.cursor()
sql = 'CREATE TABLE IF NOT EXISTS lagou (' +\
      'id int(11) NOT NULL, '+\
      'companyFullName VARCHAR(255)  NULL, '+\
      'companyShortName VARCHAR(255) NULL, '+\
      'city VARCHAR(255) NULL, '+\
      'district VARCHAR(255) NULL, '+\
      'companySize VARCHAR(255) NULL, '+\
      'financeStage VARCHAR(255) NULL, '+\
      'industryField VARCHAR(255) NULL, '+\
      'companyLabelList VARCHAR(255) NULL, '+\
      'positionName VARCHAR(255) NULL, '+\
      'positionLables VARCHAR(255) NULL, '+\
      'firstType VARCHAR(255) NULL, '+\
      'salary VARCHAR(255) NULL, '+\
      'workYear VARCHAR(255) NULL, '+\
      'education VARCHAR(255) NULL, '+\
      'jobNature VARCHAR(255) NULL, '+  \
      'positionAdvantage VARCHAR(255) NULL, '+  \
      'PRIMARY KEY (id))'
cursor.execute(sql)


def get_json(url,num):  
    headers = {  
            'User-Agent':'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/66.0.3359.139 Safari/537.36',  
            'Host':'www.lagou.com',                         
            'Referer':'https://www.lagou.com/jobs/list_%E6%95%B0%E6%8D%AE%E5%88%86%E6%9E%90?px=default&city=%E5%85%A8%E5%9B%BD',  
            'X-Anit-Forge-Code':'0',  
            'X-Anit-Forge-Token': 'None',  
            'X-Requested-With':'XMLHttpRequest'  
            }  

    data = {  
            'first': 'true',  
            'pn':num,  
            'kd':'数据分析'}  
    data = requests.post(url, headers = headers, data = data)  
    data.raise_for_status()  
    data.encoding = 'utf-8'  
    # 得到包含职位信息的字典  
    page = data.json()  
    return page  


def get_page_num(count):   
    data = math.ceil(count/15)   
    if data > 100:  
        return 100  
    else:  
        return data  

def get_page_info(jobs_list,id):  
    for job in jobs_list:  
        id = id + 1
        companyFullName = job['companyFullName']
        companyShortName = job['companyShortName']
        city = job['city']
        district = job['district']
        companySize = job['companySize']
        financeStage = job['financeStage']
        industryField = job['industryField']
        companyLabelList = ','.join(job['companyLabelList'])
        positionName = job['positionName']
        positionLables = ','.join(job['positionLables'])
        firstType = job['firstType']
        salary = job['salary']
        workYear = job['workYear']
        education = job['education']
        jobNature = job['jobNature']
        positionAdvantage = job['positionAdvantage']
        cursor.execute("INSERT INTO lagou(id,companyFullName,companyShortName,city,district,companySize,financeStage,industryField,companyLabelList,positionName,positionLables,firstType,salary,workYear,education,jobNature,positionAdvantage) values(%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s)" ,(str(id),companyFullName,companyShortName,city,district,companySize,financeStage,industryField,companyLabelList,positionName,positionLables,firstType,salary,workYear,education,jobNature,positionAdvantage))
        db.commit()
    return id

def main():  
           
    url = 'https://www.lagou.com/jobs/positionAjax.json?px=default&needAddtionalResult=false'     
    # 先设定页数为1,获取总的职位数  
    page_1 = get_json(url,1)  
    total_count = page_1['content']['positionResult']['totalCount']  
    num = get_page_num(total_count)  
    time.sleep(20)  
    print('职位总数:{},页数:{}'.format(total_count,num))  
    m = 0
    for n in range(0,101):  
        # 对每个网页读取JSON, 获取每页数据  
        page = get_json(url,n)  
        jobs_list = page['content']['positionResult']['result']  
        sum = get_page_info(jobs_list,m)  
        m = m + 15
        print('已经抓取第{}页, 职位总数:{}'.format(n, sum))  
        time.sleep(20)   
    cursor.close()
    db.close()

if __name__== "__main__":   
    main()  
