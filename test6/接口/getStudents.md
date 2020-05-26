

# 接口：getStudents  [返回](../README.md)
用例： [学生列表](../用例/学生列表.md)

- 权限：
    老师：可以看到experiment，usual，result_sum，WEB_SUM。
    
- 功能：
    返回所有学生的列表。

- API请求地址：
   接口基本地址/v1/api/getStudents

- 请求方式 ：
    GET

- 请求参数说明:
    无

- 返回实例：

        {
            "status": true,
            "info": null,
            "total": 121,
            "data": [
                {"WEB_SUM": "Y",
                "result_sum": "100",
                "GITHUB_USERNAME": "Chinajuedui",
                "STUDENT_ID": "201710414107",
                "CLASS": "软件(本)17-1",
                "NAME": "张三",
                "UPDATE_DATE": "2020-04-02 13:48:01"},
                {
                ...其他学生
                }
            ]
        }

- 返回参数说明：

  |参数名称|说明|
  |:---------:|:--------------------------------------------------------|
  |status|bool类型，true表示正确的返回，false表示有错误|
  |info|返回结果说明信息|
  |total|返回学生人数|
  |data|所有学生的数组|
  |WEB_SUM|网址是否正常|
  |RESULT_SUM|成绩的汇总|
  |GITHUB_USERNAME|GITHUB 用户名|
  |STUDENT_ID|学号|
  |CLASS|班级|
  |NAME|真实姓名|
  |UPDATE_DATE|GitHUB用户名修改日期|