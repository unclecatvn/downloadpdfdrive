# Download File PDF Google Drive Bị Chặn

- Khó chịu
1. Khi thấy một file PDF google drive bị chặn?
2. Tài liệu mình muốn tải nhưng không thể làm cách nào có thể tải về?
3. Lên mạng search "tải file pdf google drive bị chặn" nhưng không được nét, chỉ là bản mờ ?
- Đến với tôi, tôi sẽ mang tất cả đến cho bạn. Dưới đây là đoạn mã javascript giúp bạn giải quyết vấn đề đó.
- Vui lòng làm theo các bước:
1. Cuộn hết toàn bộ trang PDF bạn muốn tải, để có thể load hết toàn bộ file của bạn.
2. Chuột phải Inspect (Sử dụng nút F12 trên bàn phím) vào console rồi copy paste code vào đó ENTER và chờ đợi kết quả.

```c
let jspdf = document.createElement("script");
jspdf.onload = function () {
    let pdfDocumentName = "Document";
    let doc;

    function generatePDF (){
        let imgTags = document.getElementsByTagName("img");
        let checkURLString = "blob:https://drive.google.com/";
        let validImgTagCounter = 0;
        for (i = 0; i < imgTags.length; i++) {

            if (imgTags[i].src.substring(0, checkURLString.length) === checkURLString){
                validImgTagCounter = validImgTagCounter + 1;
                //console.log(imgTags[i].src);
                let img = imgTags[i];

                let canvas = document.createElement('canvas');
                let context = canvas.getContext("2d");
                canvas.width = img.naturalWidth;
                canvas.height = img.naturalHeight;
                //console.log("Width: " + img.naturalWidth + ", Height: " + img.naturalHeight);
                context.drawImage(img, 0, 0, img.naturalWidth, img.naturalHeight);
                let imgDataURL = canvas.toDataURL();
               // console.log(imgDataURL);

                //let ratio;
                let orientation;
                if (img.naturalWidth > img.naturalHeight){
                    //console.log("Landscape");
                    orientation = "l";
                    //ratio = img.naturalWidth/img.naturalHeight
                }else {
                    //console.log("Portrait");
                    orientation = "p";
                    //ratio = img.naturalWidth/img.naturalHeight
                }

                let scalefactor = 1.335;
                let pageWidth = img.naturalWidth * scalefactor;
                let pageHeight = img.naturalHeight * scalefactor;
               //let imagexLeft = (pageWidth - img.naturalWidth)/2;
               //let imagexTop = (pageHeight - img.naturalHeight)/2;
                if (validImgTagCounter === 1){
                    doc = new jsPDF({
                        orientation: orientation,
                        unit: "px",
                        format: [pageWidth, pageHeight],
                    });
                    doc.addImage(imgDataURL, "PNG", 0, 0, img.naturalWidth, img.naturalHeight);
                }else{
                    doc.addPage([pageWidth, pageHeight] , orientation);
                    doc.addImage(imgDataURL, "PNG", 0, 0, img.naturalWidth, img.naturalHeight);
                }
            }
        }

        pdfDocumentName = pdfDocumentName + ".pdf";
       doc.save(pdfDocumentName);
    }

    let allElements = document.querySelectorAll("*");
    let chosenElement;
    let heightOfScrollableElement = 0;

    for (i = 0; i < allElements.length; i++) {
        if ( allElements[i].scrollHeight>=allElements[i].clientHeight){
            if (heightOfScrollableElement < allElements[i].scrollHeight){
                //console.log(allElements[i]);
                //console.log(allElements[i].scrollHeight);
                heightOfScrollableElement = allElements[i].scrollHeight;
                chosenElement = allElements[i];
            }
        }
    }

    if (chosenElement.scrollHeight > chosenElement.clientHeight){
        console.log("Auto Scroll");

        let scrollDistance = Math.round(chosenElement.clientHeight/2);
        //console.log("scrollHeight: " + chosenElement.scrollHeight);
        //console.log("scrollDistance: " + scrollDistance);

        let loopCounter = 0;
        function myLoop(remainingHeightToScroll, scrollToLocation) {
            loopCounter = loopCounter+1;
            console.log(loopCounter);

            setTimeout(function() {
                if (remainingHeightToScroll === 0){
                    scrollToLocation = scrollDistance;
                    chosenElement.scrollTo(0, scrollToLocation);
                    remainingHeightToScroll = chosenElement.scrollHeight - scrollDistance;
                }else{
                    scrollToLocation = scrollToLocation + scrollDistance ;
                    chosenElement.scrollTo(0, scrollToLocation);
                    remainingHeightToScroll = remainingHeightToScroll - scrollDistance;
                }

                if (remainingHeightToScroll >= chosenElement.clientHeight){
                    myLoop(remainingHeightToScroll, scrollToLocation)
                }else{
                    setTimeout(function() {
                        generatePDF();
                    }, 1500)
                }

            }, 500)
        }
        myLoop(0, 0);

    }else{
        console.log("No Scroll");
        setTimeout(function() {
            generatePDF();
        }, 1500)
    }

};
jspdf.src = 'https://cdnjs.cloudflare.com/ajax/libs/jspdf/1.5.3/jspdf.debug.js';
document.body.appendChild(jspdf);
```
- Các bạn có thể sử dụng mã này
```c
let jspdf = document.createElement("script");
jspdf.onload = function () {
  let pdf = new jsPDF();
  let elements = document.getElementsByTagName("img");
  for (let i in elements) {
    let img = elements[i];
    console.log("add img ", img);
    if (!/^blob:/.test(img.src)) {
      console.log("invalid src");
      continue;
    }
    let can = document.createElement('canvas');
    let con = can.getContext("2d");
    con.imageSmoothingEnabled = false; // added line
    can.width = img.width;
    can.height = img.height;
    con.drawImage(img, 0, 0, img.width, img.height);
    let imgData = can.toDataURL("image/jpeg", 1.0);
    pdf.addImage(imgData, 'JPEG', 0, 0);
    pdf.addPage();
  }
  pdf.save("download.pdf");
};
jspdf.src = 'https://cdnjs.cloudflare.com/ajax/libs/jspdf/1.5.3/jspdf.debug.js';
document.body.appendChild(jspdf);
```

