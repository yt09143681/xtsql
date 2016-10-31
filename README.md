查询用户test1可以查看的页面（Sys_menu）：
步骤：
1.在cf_role中查找LoginName为test1的用户的UserID
2.在cf_userrole中查找test1的角色的RoleID
3.cf_privilege与sys_menu进行左连接
4.在cf_privilege中遍历， PrivilegeAccess='Sys_Menu' 、 PrivilegeOperation='Permit'且if PrivilegeMaster字段且if PrivilegeMaster字段
  1）如果是CF_User,则判断PrivilegeMasterKey是否等于在第一步中查找的UserID,是就并入查询结果集
  2）如果PrivilegeMaster字段是CF_Role,则判断PrivilegeMasterKey是否等于在第二步中查找的RoleID,是就并入查询结果集
查询代码：select DISTINCT M.MenuID,M.MenuNo,M.MenuName from cf_privilege P left join sys_menu M 
on P.PrivilegeAccessKey=M.MenuID,cf_user U 
where ( (P.PrivilegeMasterKey=U.UserID and U.LoginName='test1' and P.PrivilegeMaster='CF_User' ) or (P.PrivilegeMaster='CF_Role' and P.PrivilegeMasterKey = (select U_R.RoleID from cf_role R,cf_user U,cf_userrole U_R where U.UserID=U_R.UserID and R.RoleID=U_R.RoleID and U.LoginName='test1') ) ) and P.PrivilegeAccess='Sys_Menu' and PrivilegeOperation='Permit';
查询结果：
![h](https://github.com/yt09143681/xtsql/blob/master/1.jpg)



查询用户test1可以对订单(order)页面中的操作权限(sys_button):
步骤：
1.在cf_role中查找LoginName为test1的用户的UserID
2.在cf_userrole中查找test1的角色的RoleID
3.cf_privilege与sys_menu进行左连接
4.在cf_privilege中遍历，P.PrivilegeAccess='Sys_Button' 、PrivilegeOperation='Permit' 、M.MenuName='订单' 、sys_Menu.MenuNo=sys_Menu.MenuNo且if PrivilegeMaster字段   
  1）如果是CF_User,则判断PrivilegeMasterKey是否等于在第一步中查找的UserID,是就并入查询结果集
  2）如果PrivilegeMaster字段是CF_Role,则判断PrivilegeMasterKey是否等于在第二步中查找的RoleID,是就并入查询结果集
查询代码:select DISTINCT B.BtnName,B.BtnID from cf_privilege P left join sys_button B 
on P.PrivilegeAccessKey=B.BtnID,cf_user U,sys_menu M 
where ( (P.PrivilegeMasterKey=U.UserID and U.LoginName='test1' and P.PrivilegeMaster='CF_User' ) or (P.PrivilegeMaster='CF_Role' and P.PrivilegeMasterKey = (select U_R.RoleID from cf_role R,cf_user U,cf_userrole U_R where U.UserID=U_R.UserID and R.RoleID=U_R.RoleID and U.LoginName='test1') ) ) and P.PrivilegeAccess='Sys_Button' and PrivilegeOperation='Permit' AND M.MenuName='订单' and B.MenuNo=M.MenuNo;
查询结果：
![h](https://github.com/yt09143681/xtsql/blob/master/2.jpg)
