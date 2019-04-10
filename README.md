# 简介
本系统目的是为了完成机器学习算法模型的在线管理功能。功能包括模型管理，模型版本管理，并自动生成模型版本api服务接口。
接口管理地址 ：http://www.xiaoyaoji.cn/share/1wI4pWAr2h 
# 项目目录介绍
docs 项目相关文件资料 <br/>
log 日志 <br/>
model 存放模型相关文件,模型pickle文件，模型报告文件<br/>
model/feature_comments/feature_names.xls  存放特征的备注信息，用于获取api调用方式帮助信息的时候，提供模型特征参数解释。核心列 '特征名称','特征含义','时间窗','类型'<br/>
pp_app app模块 <br/>
pp_blueprint 蓝本模块 <br/>
pp_decorator 装饰器模块 <br/>
pp_entity 实体模块 <br/>
pp_test 测试模块 <br/>
pp_util 工具模块 <br/>
readme.md 说明文件 <br/>
run.py 启动入口 <br/>
start_service.sh linux linux服务器服务启动脚本 <br/>
# 部署运行
## 配置项目所需环境参数
配置模块pp_app下的app_config.py文件。配置相关数据库，缓存等参数。
### 开发环境部署
直接导入开发工具,pycharm spyder等,运行run.py文件即可
### linux 生产部署 
1 复制项目到linux系统，修改start_service.sh ANACONDA_PATH 参数 <br/>
2 为了避免脚本字符编码导致的无法再linux下直接运行，建议在服务器中另行将脚本中代码复制出来，重新创建该文件。<br/>
3 设置可执行权限 chmod +x start_service.sh <br/>
4 使用命令 ./start_service.sh start|stop|restart 执行命令 ，而非 sh start_service.sh start|stop|restart <br/>

# 模型文件备注
## 模型文件要求
### 目前支持的模型类xgboost.core.Booster
训练模型时候务必提供特征名称.推荐使用dataframe生成训练数据,模型文件生成请使用pickle序列化.
如下代码：<br/>
import xgboost as xgb;<br/>
import pickle ;<br/>
import pandas as pd<br/>
df = pd.read_csv(...)<br/>
train_dmx = xgb.DMatrix(df[feature_cols],label=df[label_col])<br/>
model_obj = xgb.train(params,train_dmx,othoer...)<br/>
f = open(f_path,'wb);<br/>
pickle.dump(model_obj);<br/>
f.close();<br/>
### 扩展支持的模型类
如需要扩展其他支持的类，则请修改 base_util.py 下的 load_model_from_file 的验证模型文件函数,以及model_api.py 下的相关函数
