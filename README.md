# tipsForJava

1. bufferedImage 90도 회전 (시계방향)

private BufferedImage rotate90(BufferedImage img, int width, int height) { 
  BufferedImage newImage = new BufferedImage(height, width, img.getType());
  Graphics2D g2 = newImage.createGraphics();
  g2.translate(height/2, width/2);
  g2.rotate(Math.toRadians(90));
  g2.translate(-width/2, -height/2);
  g2.setBackground(Color.WHITE);
  g2.clearRect(0, 0, width, height);
  g2.drawImage(img,0,0,null);
  g2.dispose();
  return newImage;  
}

2. front에서 daraURL로 보내온 BASE64 이미지 ZPL로 바꿔서 리턴

public String dataUrl2Zpl(String dataUrl) {
    System.out.println("dataUrl ====>" +dataUrl);
    
    /* dataURL 헤더 제거*/
		dataUrl = dataUrl.replace("data:image/png;base64,", "");
		byte[] imageData = Base64.decodeBase64(dataUrl);
		
    /*ZPL코드로 바꿀 변수 세팅*/
		BufferedImage originalImage = null;
		ByteArrayInputStream bis = null;
		String imageCode = null;
		
		try {	
		bis = new ByteArrayInputStream(imageData);
		originalImage = ImageIO.read(bis);
		imageCode = convertfromImg(originalImage);
		}catch(Exception e) {
			e.printStackTrace();
		}finally {
			try {
				bis.close();
			}catch(IOException e) {
				e.printStackTrace();
			}
		}
			
   	return imageCode;
}
