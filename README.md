查询用户test1可以查看的页面（Sys_menu）：
查询代码：select DISTINCT M.MenuID,M.MenuNo,M.MenuName from cf_privilege P left join sys_menu M 
on P.PrivilegeAccessKey=M.MenuID,cf_user U 
where ( (P.PrivilegeMasterKey=U.UserID and U.LoginName='test1' and P.PrivilegeMaster='CF_User' ) or (P.PrivilegeMaster='CF_Role' and P.PrivilegeMasterKey = (select U_R.RoleID from cf_role R,cf_user U,cf_userrole U_R where U.UserID=U_R.UserID and R.RoleID=U_R.RoleID and U.LoginName='test1') ) ) and P.PrivilegeAccess='Sys_Menu' and PrivilegeOperation='Permit';
查询结果：



查询用户test1可以对订单(order)页面中的操作权限(sys_button):
查询代码:select DISTINCT B.BtnName,B.BtnID from cf_privilege P left join sys_button B 
on P.PrivilegeAccessKey=B.BtnID,cf_user U,sys_menu M 
where ( (P.PrivilegeMasterKey=U.UserID and U.LoginName='test1' and P.PrivilegeMaster='CF_User' ) or (P.PrivilegeMaster='CF_Role' and P.PrivilegeMasterKey = (select U_R.RoleID from cf_role R,cf_user U,cf_userrole U_R where U.UserID=U_R.UserID and R.RoleID=U_R.RoleID and U.LoginName='test1') ) ) and P.PrivilegeAccess='Sys_Button' and PrivilegeOperation='Permit' AND M.MenuName='订单' and B.MenuNo=M.MenuNo;
查询结果：
