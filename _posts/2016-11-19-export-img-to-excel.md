---
layout: post
title: 导出图片到Excel
description: 这里的图片是用ECharts框架制作的web图表,准确来说是导出Base64编码的图片
keyword: echart导出,导出图片到Excel
---

### 导出数据和图片到Excel用得是POI,我自己写过一些代码,也收藏有demo,去stackOverf找也有很多

* 今天说说怎么把图片导出去

	
	
	/**
	 *  把以base64格式编码的图片写到excel中
	 * @param base64Str 被写出的base64格式的图表
	 * @param work  目标工作薄
	 * @param sheet 目标表格
	 * @param rowIndex 当前处于目标表格的行
	 */
	public static void writeBase64ImageToExcel(String base64Str,Workbook work,Sheet sheet,int rowIndex ){
		try{ 
			// 对经过base64编码的字符串进行解码
			byte[] dataArray = new BASE64Decoder().decodeBuffer(base64Str);

			// add a picture tu workbook
			int pictureIdx = work.addPicture(dataArray, Workbook.PICTURE_TYPE_PNG);
			// instantiating concrete classes
			CreationHelper helper = work.getCreationHelper();

			// create the top-level drawing patriarch
			Drawing drawing = sheet.createDrawingPatriarch();

			// create an anchor that is attarched to the workbook
			ClientAnchor anchor = helper.createClientAnchor();

			// create an anchor with upper left cell and bottom right cell
			anchor.setCol1(1);
			anchor.setRow1(rowIndex + 6);
			anchor.setCol2(6);
			anchor.setRow2(rowIndex + 20);

			// create picture
			Picture picture = drawing.createPicture(anchor, pictureIdx);
			picture.resize();
		} catch (IOException e) {
			e.printStackTrace();
			
		}
	}