# Train-scheduling-problem

Train scheduling problem

【参考程序】

【算法一】const max=10;

type shuzu=array[1..max] of 0..max;

var   stack,exitout:shuzu;

      n,total:integer;
      
procedure output(exitout:shuzu);

var i:integer;

begin

     for i:=1 to n do write(exitout[i]:2);writeln;
     
     inc(total);
     
end;

procedure find(dep,have,rest,exit_weizhi:integer;stack,exitout:shuzu);

{dep:步数,have:入口处有多少辆车;rest:车站中有多少车;}

{exit_weizhi:从车站开出后,排在出口处的位置;}

{stack:车站中车辆情况数组;exitout:出口处车辆情况数组}

var i:integer;

begin  {分入站,出站两种情况讨论}

	    if have>0 then begin    {还有车未入站}
      
	       stack[rest+1]:=n+1-have;   {入站}
         
	       if dep=2*n then output(exitout)
         
		  else find(dep+1,have-1,rest+1,exit_weizhi,stack,exitout);
      
	    end;
      
	    if rest>0 then begin    {还有车可出站}
      
	       exitout[exit_weizhi+1]:=stack[rest];   {出站}
         
	       if dep=2*n then output(exitout)	{经过2n步后,输出一种方案}
         
		  else find(dep+1,have,rest-1,exit_weizhi+1,stack,exitout);
      
	   end;
     
end;

begin

     writeln('input n:');
     
     readln(n);
     
     fillchar(stack,sizeof(stack),0);
     
     fillchar(exitout,sizeof(exitout),0);
     
     total:=0;
     
     find(1,n,0,0,stack,exitout);
     
     writeln('total:',total);
     
     readln;
     
end.

【解法2】用穷举二进制数串的方法完成.

uses crt;

var i,n,m,t:integer;

    a,s,c:array[1..1000] of integer;
    
procedure test;

var t1,t2,k:integer;

    notok:boolean;
    
begin

     t1:=0;k:=0;t2:=0;
     
     i:=0;
     
     notok:=false;
     
     repeat   {二进制数串中,0表示出栈,1表示入栈}
     
           i:=i+1; {数串中第I位}
           
           if a[i]=1 then begin {第I位为1,则表示车要入栈}
           
              inc(k); {栈中车数}
              
              inc(t1); {入栈记录,T1为栈指针,S为栈数组}
              
              s[t1]:=k;
              
            end
            
           else {第I位为0,车要出栈}
           
             if t1<1 then notok:=true {已经无车可出,当然NOT_OK了}
             
                        else begin inc(t2);c[t2]:=s[t1];dec(t1);end;
                        
                       {栈中有车,出栈,放到C数组中去,T2为C的指针,栈指针T1下调1}
                       
     until (i=2*n) or notok; {整个数串均已判完,或中途出现不OK的情况}
     
     if (t1=0) and not notok then begin  {该数串符合出入栈的规律则输出}
     
        inc(m);write('[',m,']');
        
        
        for i:=1 to t2 do write(c[i]:2);
        
        writeln;
        
     end;
     
end;

begin

     clrscr; write('N=');readln(n);
     
     m:=0;
     
     for i:=1 to 2*n do a[i]:=0; {
     
     
     repeat {循环产生N位二进制数串}
     
           test;   {判断该数串是否符合车出入栈的规律}
           
           t:=2*n;
           
           a[t]:=a[t]+1; {产生下一个二进制数串}
           
           while (t>1) and (a[t]>1) do begin
           
                 a[t]:=0;dec(t);a[t]:=a[t]+1;
                 
           end;
           
     until a[1]=2;
     
     readln;
     
end.

N:       4        6        7         8

TOTAL:  14       132      429       1430
