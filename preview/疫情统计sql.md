- 企业提交申请统计

  ```sql
  SELECT
  	count( * ) AS total 
  FROM
  	t_outbreak AS t1
  	LEFT JOIN t_company_jiangjin AS t2 ON t1.employer = t2.company_name 
  WHERE
  	FIND_IN_SET( 'outbreak', t1.form_type ) 
  	AND ( t1.employer != "无" OR t1.employer IS NOT NULL ) 
  	AND t2.industry_code IS NOT NULL 
  	AND t1.created_at <= "2020-03-13 23:59:59"
  	AND t2.industry_code >= 8411 AND t2.industry_code <= 8529
  	
  	
  	# 公司重复
  	select MAX(id),count(*) as total,company_name from t_company_jiangjin GROUP BY company_name HAVING total > 1;
  # 删除重复公司
  DELETE 
  FROM
  	t_company_jiangjin 
  WHERE
  	id IN (
  	SELECT
  		id 
  	FROM
  	( SELECT MAX( id ) AS id, count( * ) AS total, company_name FROM t_company_jiangjin GROUP BY company_name HAVING total > 1 ) AS t2 
  	);
  ```

  