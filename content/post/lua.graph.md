---
title: "Lua画图软件"
date: "2013-07-15"
tags:
  - "Lua"
  - "graph"
  - "script"
---

在网上看到一篇文章 [用Lua画二元方程](http://www.cnblogs.com/rufi/archive/2011/06/28/GraphPlot.html)，很有意思。 <!--more-->

作者提供了源代码，很是佩服Lua的个功能强大和作者的奇思妙想。

这里仅提供完整的代码封装一份,不依赖外部DLL，成为一个独立的Win32应用程序:
[下载链接](/assets/media/graph.exe)

	setmetatable(_G, { __index = math })
	e = exp(1)

	cfg=[[
	Config={Width=400, Height=400, Xmin=-100, Xmax=100, Ymin=-100, Ymax=100, Split = 4, Expr="abs(x - y) % 20 < 0.0001"}

	function between(x, minValue, maxValue)
	  return x >= minValue and x <= maxValue
	end

	function belongto(x, ranges)
	 for _,range in ipairs(ranges) do
	      if between(x, range[1], range[2]) then return true end
	 end
	 return false
	end

	Favorites={
	branchname = "Favorites",
	{leafname = "axis", tip = "x==0 or y==0", config = {Xmin=-100, Xmax=100, Ymin=-100,Ymax=100,Split=4}},
	{leafname = "rect", tip = "abs(x+50)<10 and abs(y-20)<20", config = {Xmin=-100, Xmax=100, Ymin=-100,Ymax=100,Split=4}},
	{leafname = "lines(V)", tip = "x%15 == 0", config = {Xmin=-100, Xmax=100, Ymin=-100,Ymax=100,Split=4}},
	{leafname = "lines(H)", tip = "y%15 == 0", config = {Xmin=-100, Xmax=100, Ymin=-100,Ymax=100,Split=4}},
	{leafname = "lines(S)", tip = "(y-x)%20 == 0", config = {Xmin=-100, Xmax=100, Ymin=-100,Ymax=100,Split=4}},
	{leafname = "lines(S)", tip = "(y+x)%20 == 0", config = {Xmin=-100, Xmax=100, Ymin=-100,Ymax=100,Split=4}},
	{leafname = "triangular", tip = "y<x+50 and x*y<0 and y>0", config = {Xmin=-100, Xmax=100, Ymin=-100,Ymax=100,Split=4}},
	{leafname = "triangular", tip = "(x>-20 and x<=0 and y<2*x+40 or x>=0 and x<20 and y<40-2*x) and y>0", config = {Xmin=-100, Xmax=100, Ymin=-100,Ymax=100,Split=4}},
	{leafname = "disk", tip = "x^2 + y^2 < 500", config = {Xmin=-100, Xmax=100, Ymin=-100,Ymax=100,Split=4}},
	{leafname = "circle", tip = "abs((x^2 + y^2)  - 1000) < 5", config = {Xmin=-100, Xmax=100, Ymin=-100,Ymax=100,Split=4}},
	{leafname = "ellipse", tip = "abs(((x/2)^2 + y^2)  - 1000) < 5", config = {Xmin=-100, Xmax=100, Ymin=-100,Ymax=100,Split=4}},
	{leafname = "sun", tip = "(x==0 or abs(atan(y/x)%0.2)<0.005) and between(x^2+y^2, 1600,4900) or x^2+y^2<900", config = {Xmin=-100, Xmax=100, Ymin=-100,Ymax=100,Split=4}},
	{leafname = "face", tip = "(x/2-25)^2+(y-50)^2<200 or (x/2+25)^2+(y-50)^2<200 or ((x>-20 and x<=0 and y<2*x+40 or x>=0 and x<20 and y<40-2*x) and y>0) or abs((x/2)^2+(y+30)^2-400)<10 or abs(y-(x/8)^2+80)<0.5", config = {Xmin=-100, Xmax=100, Ymin=-100,Ymax=100,Split=4}},
	{leafname = "circles", tip = "sqrt(x^2 + y^2) % 10 < 0.1", config = {Xmin=-100, Xmax=100, Ymin=-100,Ymax=100,Split=4}},
	{leafname = "wave", tip = "(x^2 + y^2) % 80 < 5", config = {Xmin=-100, Xmax=100, Ymin=-100,Ymax=100,Split=4}},
	{leafname = "sin", tip = "abs(y-20*sin(x/5)) < 0.1", config = {Xmin=-100, Xmax=100, Ymin=-100,Ymax=100,Split=4}},
	{leafname = "chessboard", tip = "sin(y/5)*cos(x/5) < 0.000001", config = {Xmin=-100, Xmax=100, Ymin=-100,Ymax=100,Split=4}},
	{leafname = "cells", tip = "sin(y/3) + sin(x/3) < 0.000001", config = {Xmin=-100, Xmax=100, Ymin=-100,Ymax=100,Split=4}},
	{leafname = "texture", tip = "sin((y/3)^2 + x/2) < 0.1", config = {Xmin=-100, Xmax=100, Ymin=-100,Ymax=100,Split=4}},
	{leafname = "texture", tip = "sin(y^2)+cos(x^2) < 0.000001", config = {Xmin=-8, Xmax=8, Ymin=-8,Ymax=8,Split=5}},
	{leafname = "spots", tip = "sin(x)^2+cos(y)^2 < 0.5", config = {Xmin=-8, Xmax=8, Ymin=-8,Ymax=8,Split=5}},
	{leafname = "Margarita", tip = "mod(sqrt(x^2+y^2)-3.5*atan(x,y)+sin(x)+cos(y),pi)<pi/2", config = {Xmin=-20, Xmax=20, Ymin=-20,Ymax=20,Split=8}},
	{leafname = "pattern", tip = "sin(x*sin(y)+y*sin(x)) < 0.001", config = {Xmin=-12, Xmax=12, Ymin=-12,Ymax=12,Split=8}},
	{leafname = "untitled", tip = "abs(e^(sin(x)+cos(y))-sin(e^(x+y)))<0.01", config = {Xmin=-10, Xmax=10, Ymin=-10,Ymax=10,Split=15}},
	{leafname = "untitled", tip = "abs( sin(sin(x)+cos(y)) - cos(sin(x*y)+cos(x)) )<0.01", config = {Xmin=-10, Xmax=10, Ymin=-10,Ymax=10,Split=10}},
	{leafname = "untitled", tip = "abs( sin(x^2+y^2) - cos(x*y) )<0.01", config = {Xmin=-10, Xmax=10, Ymin=-10,Ymax=10,Split=10}},
	{leafname = "mesh", tip = "abs( sin(x^2-y^2) - cos(x*y) )<0.01", config = {Xmin=-10, Xmax=10, Ymin=-10,Ymax=10,Split=10}},
	{leafname = "untitled", tip = "abs( sin(x^sin(y)+y^cos(x))-cos(y^x+x^y))<0.005", config = {Xmin=0, Xmax=20, Ymin=0,Ymax=20,Split=10}},
	{leafname = "Frontispiece", tip = "abs( x/sin(x)+y/sin(y) - x*y/sin(x*y) )<1", config = {Xmin=-10, Xmax=10, Ymin=-10,Ymax=10,Split=10}},
	{leafname = "Threesome", tip = "belongto(sin(sqrt((x+5)^2+y^2))*cos(8*atan(y/(x+5)))*sin(sqrt((x-5)^2+(y-5)^2))*cos(8*atan((y-5)/(x-5)))*sin(sqrt(x^2+(y+5)^2))*cos(8*atan((y+5)/x)), {{-0.1,0},{0.2,math.huge}})", config = {Xmin=-10, Xmax=10, Ymin=-10,Ymax=10,Split=10}},
	{leafname = "TaiChi", tip = "x^2+y^2<64 and x^2+(y-4)^2>16 and x^2+(y+4)^2>1 and x<=0 or between(x^2+(y+4)^2,1,16) or x^2+(y-4)^2<1 or abs(sqrt(x^2+y^2)-8)<0.005", config = {Xmin=-10, Xmax=10, Ymin=-10,Ymax=10,Split=10}},
	}
	]]

	-- Load Config and Favorites
	-- dofile("Favorites.lua")
	-- make sure to load the "config files"
	require("io")
	ferr = io.open("Favorites.lua")
	if not ferr then
	  assert(loadstring(cfg))()
	else
	  dofile("Favorites.lua")
	  io.close(ferr)
	end
	pixels = {}
	for r=1,Config.Height do
	 pixels[r] = {}
	 for c=1,Config.Width do
	       pixels[r][c] = false
	 end
	end

	cnv = iup.canvas{ rastersize=string.format("%dx%d", Config.Width, Config.Height) }
	treeFavorite = iup.tree{ size = "100x100"}

	canvasColor = cd.EncodeColor(255,255,255)
	penColor = cd.EncodeColor(0,0,0)

	function selectColor(self)
	 r,g,b = iup.GetColor(iup.CENTER,iup.CENTER)
	self.bgcolor = string.format("%d %d %d",r,g,b)
	if self.name == "btnPen" then penColor = cd.EncodeColor(r,g,b)
	else canvasColor = cd.EncodeColor(r,g,b) end
	end

	dlg = iup.dialog
	{
	   title="Plot graphs of equations and inequalities in two vairables",
	   resize="NO",
	   minbox="NO",
	   iup.hbox
	   {
	       iup.vbox
	       {
	          iup.frame
	          {
	              title = "Color",
	              iup.hbox
	              {
	                   iup.label{title = " Canvas: "},
	                   iup.button{ name = "btnCanvas", size = "22x10", bgcolor="255 255 255", flat="YES", action = selectColor},
	                   iup.label{title = " Pen: "},
	                   iup.button{ name = "btnPen", size = "22x10", bgcolor="0 0 0", flat="YES", action = selectColor},
	                   alignment = "ACENTER"
	              }
	          },
	          iup.frame
	          {
	              title = "Range",
	              iup.vbox
	              {
	                   iup.hbox
	                   {
	                       iup.label{title = "X: "},
	                       iup.text{ name = "txtXMin", size = "38x10", value = Config.Xmin},
	                       iup.label{title = " ~ "},
	                       iup.text{ name = "txtXMax", size = "38x10", value = Config.Xmax}
	                   },
	                   iup.hbox
	                   {
	                       iup.label{title = "Y: "},
	                       iup.text{ name = "txtYMin", size = "38x10", value = Config.Ymin},
	                       iup.label{title = " ~ "},
	                       iup.text{ name = "txtYMax", size = "38x10", value = Config.Ymax}
	                   }
	              }
	          },
	          iup.hbox
	          {
	               iup.label{ title = "Pixel Split: " },
	               iup.text{ name = "txtSplit", value = Config.Split},
	               alignment = "ACENTER"
	          },
	          treeFavorite,
	          margin = "2x2"
	       },
	       iup.vbox
	       {
	           iup.hbox
	           {
	               iup.label{title = "Expression: "},
	               iup.text{ name = "exprtext", rastersize = (Config.Width-120).."x22", value = "abs(x - y) % 20 < 0.0001"},
	               iup.button{title = "Plot", size = "34x12", action = function() plotExpression() end},
	               alignment = "ACENTER"
	           },
	           cnv,
	           margin = "2x2"
	       }
	   }
	}

	function getNumber(name)
	   return tonumber(iup.GetDialogChild(dlg, name).value)
	end

	function setTextValue(name, value)
	   iup.GetDialogChild(dlg, name).value = value
	end

	function plotExpression()
	   local expr = iup.GetDialogChild(dlg, "exprtext").value
	   local func, err = loadstring("return function (x,y) return ".. expr.." end")
	   if func==nil then iup.Message("Error", "Expression has syntax errors:\r\n"..err) return end
	   equation = func()
	   drawing = true
	   iup.Redraw(cnv, 0)
	end

	function cnv:map_cb()
	 canvas = cd.CreateCanvas(cd.IUP, self)
	end

	drawing = false
	function cnv:action()
	 canvas:Activate()
	 canvas:Background(canvasColor)
	 canvas:Clear()
	 local width = Config.Width
	 local height = Config.Height
	 if drawing == false then
	   for r=height,1,-1 do
	     for c=1,width do
	         if pixels[r][c] then
	           canvas:Pixel(c, r, penColor)
	         end
	     end
	   end
	   return
	 end

	 dlg.active = "NO"
	 local xMin = getNumber("txtXMin")
	 local xMax = getNumber("txtXMax")
	 local yMin = getNumber("txtYMin")
	 local yMax = getNumber("txtYMax")
	 local split = getNumber("txtSplit")
	 local DX = (xMax - xMin)/width
	 local DY = (yMax - yMin)/height
	 local dx = DX / split
	 local dy = DY / split
	 local x = xMin
	 local y = yMax
	 for r=height,1,-1 do
	     for c=1,width do
	       local fill = isSolution(x,y,dx,dy,split)
	       pixels[r][c] = fill
	       if fill then
	           canvas:Pixel(c, r, penColor)
	       end
	       x = x + DX
	     end
	     x = xMin
	     y = y - DY
	 end
	 drawing = false
	 dlg.active = "YES"
	end

	function isSolution(x0,y0,dx,dy,split)
	   local x = x0
	   local y = y0
	   for i=1,split do
	       for j=1,split do
	           if equation(x, y) then return true end
	           x = x + dx
	       end
	       x = x0
	       y = y - dy
	   end
	   return false
	end

	function treeFavorite:selection_cb(id, status)
	   if status == 1 then
	       node = Favorites[id]
	       setTextValue("exprtext", node.tip)
	       if node.config ~= nil then
	           setTextValue("txtXMin", node.config.Xmin)
	           setTextValue("txtXMax", node.config.Xmax)
	           setTextValue("txtYMin", node.config.Ymin)
	           setTextValue("txtYMax", node.config.Ymax)
	           setTextValue("txtSplit", node.config.Split)
	       end
	   end
	end

	function dlg:close_cb()
	 canvas:Kill()
	 self:destroy()
	 return iup.IGNORE
	end

	dlg:show()
	iup.TreeAddNodes(treeFavorite, Favorites)
	iup.MainLoop()
