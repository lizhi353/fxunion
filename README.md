# fxunion

1. redis使用

   ```go
     server := "192.168.0.32:6379"
   	pwd := "Dearccbj2018@02"//如无密码，请放空字符串
   //建立redis链接
   	redisPool := redis.InitRedis(server, pwd)
   //获取redis客户端对象
   	redisClient := redigopack.InitRedisClient(redisPool)
   //关闭对象dia对象
   	defer redisPool.ConnPool.Close()
   //尝试给aa这个键赋值
   	redisClient.String.Set("aa",22211, int64(60))
   //获取aa的值
     aa,_ := redisClient.String.Get("aa").String()
     fmt.Printf("redis key aa value:%s\n", aa)
   ```

   

2. 分页使用

   ```go
   func MapList(ctx *gin.Context)  {
   	var newList []*HomeList
   	query := model.GetNewQueryByTypeId()
   	page := ctx.Query("page")
   	pageInt,_ := strconv.Atoi(page)
   	paginationObj := pagination.InitPagination(10, pageInt,  query)//加载分页,参数： pagesize、当前page、gorm的*gorm.DB
   	err := paginationObj.GetList(&newList)//绑定列表
   	if err != nil {
   		logrus.Error(err)
   		ResponseException("加载列表失败", ctx)
   	}
   	ctx.JSON(http.StatusOK, gin.H{"newList":newList,"pageCount":paginationObj.GetPageCount(), "page": paginationObj.GetPage()})
   }
   ```
