# DANeS - Bộ dữ liệu nguồn mở các đầu báo điện tử
![12613](https://user-images.githubusercontent.com/94349957/143620522-2b417ece-2482-4475-a261-120af096cb0d.jpg)
*<sub>Nguồn: <a href="https://www.freepik.com/vectors/technology">Technology vector created by macrovector - www.freepik.com</a>.</sub>*

DANeS là một bộ dữ liệu mở xây dựng dựa trên sự hợp tác của DATASET. JSC và AIV Group. Bộ dữ liệu gồm ~ 500.000 bài báo điện tử tiếng Việt đến từ các trang báo như: tuoitre.vn, baobinhduong.vn, baoquangbinh.vn, kinhtechungkhoan.vn, doanhnghiep.vn, vnexpress.net, ... Các bài báo sẽ bao gồm tiêu đề, URL, mô tả tổng quan từng bài báo và được dán nhãn tích cực/tiêu cực/trung tính dựa trên nội dung tiêu đề.

DANeS được đưa ra để phục vụ cộng đồng và các dự án AI tại Việt Nam, với hy vọng thúc đẩy phong trào kiến tạo các bộ dữ liệu mở để giải quyết các bài toán chung của xã hội. Kho dữ liệu tập hợp số lượng lớn các đầu báo để hỗ trợ huấn luyện mô hình AI phân biệt được sắc thái văn bản dựa trên các cấp khác nhau. Bạn có thể chia sẻ dự án/ sản phẩm sử dụng mô hình và kho dữ liệu của DANeS với chúng chúng tôi qua email: info@dataset.vn


<!-- TABLE OF CONTENTS -->
## Mục lục
  <ol>
    <li><a href="#cây-thư-mục">Cây thư mục</a></li>
    <li><a href="#định-dạng-dữ-liệu">Định dạng dữ liệu</a>
    <li><a href="#quy-trình-dán-nhãn">Quy trình dán nhãn</a></li>
    <li><a href="#quy-trình-review">Quy trình review</a></li>
    <li><a href="#quy-trình-cập-nhật">Quy trình cập nhật</a></li>
    <li><a href="#bản-quyền">Bản quyền</a></li>
    <li><a href="#về-chúng-tôi">Về chúng tôi</a></li>
  </ol>
</details>

## Cây thư mục
	DANeS
	  |
	  |____README.md
	  |
	  |____raw_data
	  |	   |____ DANeS_batch_#1.json
	  |	   |____ DANeS_batch_#2.json
	  |	   |____ DANeS_batch_#3.json
	  |	   |____ DANeS_batch_#4.json
	  |	   |____ DANeS_batch_#5.json
	  |	   |____ DANeS_batch_#6.json
	  |	   |____ DANeS_batch_#7.json
	  |	   |____ DANeS_batch_#8.json
	  |	   |____ README.md
	  |
	  |____annotated_data
	  |	   |____ #contains annotated data
	  |
	  |____model
		   |____ Train_opensource.py
		   |____ README.md
		   |____ LICENSE
 
## Định dạng dữ liệu
Dữ liệu thô được lưu trữ trong thư mục raw_data dưới định dạng là tệp tin [`.json`](https://www.json.org) và được chia ra làm 8 batch. Mỗi batch bao gồm 1 mảng chứa nhiều json và mỗi json là 1 bản ghi của bộ dữ liệu. 

| Key          | Type                   | Description                                  |
| ------------ | -----------------------| -------------------------------------------- |
| text         | string                 | title of the digital news                    |
| meta         | json                   | metadata of the digital news                 |
| uri          | string                 | link to the digital news                     |
| description  | string                 | description of the digital news              |

Dưới đây là ví dụ về định dạng của mỗi bản ghi:
```javascript
{
        "text": "Ba ra đi vào ngày nhận điểm thi, nữ sinh được hỗ trợ học phí",
        "meta": {
            		"description": "Ngày nhận được tin đỗ đại học cũng là lúc bố mất vì Covid-19, L.A dường như gục ngã. Thế nhưng, bên cạnh em đã có các mạnh thường quân hỏi han, hỗ trợ về kinh tế.",
            		"uri": "https://yan.vn/ba-ra-di-vao-ngay-nhan-diem-thi-nu-sinh-duoc-ho-tro-hoc-phi-277328.html"
        	}
}
``` 

Dữ liệu đã được gán nhãn được lưu trữ trong thư mục annotated_data dưới định dạng là tệp tin [`.json`](https://www.json.org) và được chia ra thành nhiều batch, cập nhật theo tháng và mỗi batch sẽ không có số lượng bản ghi cố định. Mỗi batch bao gồm 1 mảng chứa nhiều json và mỗi json và 1 bản ghi của bộ dữ liệu đã gán nhãn.

| Key          | Type                   |        Include             | Description                                  |
| ------------ | -----------------------| -------------------------- | -------------------------------------------- |
| id           | string                 |         none               | id of each instance                          |
| annotations  | array                  |          id                | id of class belong to specific instance      |
|              |                        |         type               | type of annotation                           |
|              |                        |         value              | value of annotation                          |
|              |                        |        to_name             | type of the value of annotation              |
|              |                        |       from_name            | name of the annotation                       |
| data         | json                   |text, meta, uri, description| contain raw data info                        |

Dưới đây là ví dụ về định dạng của mỗi bản ghi:
```javascript
{
        "id": 785436,
        "annotations": [
            {
                "id": "Eju0SNkpeb",
                "type": "choices",
                "value": {
                    "choices": [
                        "Trung tính"
                    ]
                },
                "to_name": "text",
                "from_name": "sentiment"
            },
            {
                "id": "Hoip8he_f6",
                "type": "choices",
                "value": {
                    "choices": [
                        "Đời sống",
                        "Xã hội",
                        "Hóng biến"
                    ]
                },
                "to_name": "text",
                "from_name": "topic"
            }
        ],
        "data": {
            "meta": {
                "uri": "https://toquoc.vn/cau-ca-nha-dai-nam-khoe-duoc-me-cho-di-choi-ngoi-trong-sieu-xe-rolls-royce-40-ty-ngam-co-ngoi-minh-se-thua-ke-trong-tuong-lai-222021299202526108.htm",
                "description": "(Tổ Quốc) - Được biết, siêu xe mà bà chủ Đại Nam lái chở cậu con trai quý tử đi chơi là chiếc Rolls-Royce Wraith thuộc thế hệ đầu tiên, giá thị trường trước đó khoảng 40 tỷ đồng"
            },
            "text": "\"Cậu cả\" nhà Đại Nam khoe được mẹ chở đi chơi, ngồi trong siêu xe Rolls-Royce 40 tỷ ngắm \"cơ ngơi\" mình sẽ thừa kế trong tương lai"
        }
    }
 ```
  
## Quy trình dán nhãn
- Bước 1: Đăng nhập.

![DANeS redo 1](https://user-images.githubusercontent.com/94349957/144919125-6c21c457-4cf0-4a07-82c4-9689ff7d613d.gif)

- Bước 2: Dán nhãn.
	+ Tiêu đề được phân loại sắc thái: tích cực, tiêu cực, trung tính.
	+ Tiêu đề được phân loại vào các chủ đề liên quan trong 22 chủ đề:Thế giới, Chính trị, Kinh tế, Thể thao, Văn hoá, Giải trí, Công nghệ, Khoa học, Giáo dục, Đời sống, Pháp luật, Bất động sản, Xã hội, Giao thông, Môi trường, Chứng khoán, Covid-19, Hóng biến, Game, Phim ảnh, Sức khoẻ, Du lịch

![DANeS redo 2](https://user-images.githubusercontent.com/94349957/144919228-fbda5c51-9b7f-47ef-a51e-ca95813419c7.gif)

## Quy trình review

- Người review và kiểm tra chéo sẽ được quản lý hoặc chủ sở hữu dự án lựa chọn từ những CTV dựa trên chất lượng công việc và thái độ trong quá trình làm việc.
- Quy trình review data gồm 2 bước: kiểm tra chéo và review
     + Mỗi người kiểm tra chéo sẽ được giao cho khoảng 20% số lượng bản ghi của người dán nhãn khác.
        => Nếu người kiểm tra chéo phát hiện được bản ghi không đạt chất lượng thì phải sửa lại để đạt đúng yêu cầu.
     + Người review, mặt khác, sẽ tiến hành check 20-50% tổng số lượng nhãn được gán của cả dự án.
        => Nếu người review phát hiện bản ghi được gán nhãn không đạt chất lượng thì có thể lựa chọn sửa lại hoặc chuyển lại cho người gán nhãn/người kiểm tra chéo gán nhãn lại.

## Quy trình cập nhật

- Bộ dữ liệu thô sẽ được cập nhật đầy đủ trên trang chính thức của DANeS tại Github. (https://github.com/dataset-vn/DANeS)

- Phần dữ liệu gồm các bản ghi đã được dán nhãn sẽ được cập hàng tháng trên trang chính thức của DANeS tại Github. (https://github.com/dataset-vn/DANeS)


## Bản quyền

<a rel="license" href="http://creativecommons.org/licenses/by/4.0/"><img alt="Giấy phép Creative Commons " style="border-width:0" src="https://i.creativecommons.org/l/by/4.0/88x31.png" /></a><br />
Phần dữ liệu được dán nhãn thuộc <a xmlns:cc="http://creativecommons.org/ns#" href="https://github.com/dataset-vn/DANeS" property="cc:attributionName" rel="cc:attributionURL">DANeS</a> được cấp phép theo <a rel="license" href="http://creativecommons.org/licenses/by/4.0/">Giấy phép Creative Commons Ghi công 4.0 Quốc tế </a>.

Với loại giấy phép này bạn có thể:
- Sao chép, chỉnh sửa, phân phối và xây dựng sản phẩm của bạn dựa trên các dữ liệu đã công bố trong dự án này ở bất kì định dạng hoặc bất kỳ phương tiện nào.
- Chỉnh sửa, biến đổi và xây dựng lại cho mọi mục đích, kể cả mục đích thương mại.
Tuy nhiên bạn cần phải trích dẫn nguồn gốc của tài liệu này khi mà bạn sử dụng bất kỳ dữ liệu đã được dán nhãn và công bố trong bộ dữ liệu DANeS này.

Nếu bạn cần trích dẫn tới bộ dữ liệu của chúng tôi, xin hãy sử dụng:
  `<a rel="license" href="http://creativecommons.org/licenses/by/4.0/"><img alt="Giấy phép Creative Commons " style="border-width:0" src="https://i.creativecommons.org/l/by/4.0/88x31.png" /></a><br />
Phần dữ liệu được dán nhãn thuộc <a xmlns:cc="http://creativecommons.org/ns#" href="https://github.com/dataset-vn/DANeS" property="cc:attributionName" rel="cc:attributionURL">DANeS</a> được cấp phép theo <a rel="license" href="http://creativecommons.org/licenses/by/4.0/">Giấy phép Creative Commons Ghi công 4.0 Quốc tế</a>.`

## Về chúng tôi

### DATASET .JSC - (+84) 98 442 0826 - info@dataset.vn

Sứ mệnh của Dataset là hỗ trợ các các cá nhân và tổ chức có nhu cầu về thu thập và xử lý dữ liệu bằng cách cung cấp công cụ giúp đơn giản hóa và tăng hiệu suất quá trình thu thập, xử lý dữ liệu của các lực lượng xử lý dữ liệu. Với hệ thống nguồn nhân lực đông đảo và chuyên nghiệp, Dataset mong muốn đưa đến cho đối tác một giải pháp toàn diện và chất lượng, phù hợp với đặc thù của thị trường Việt Nam.

Website: [Dataset.vn](http://dataset.vn)

LinkedIn: [Dataset.vn - Data Crowdsourcing Platform](https://www.linkedin.com/company/dataset-vn/about/)

Facebook: [Dataset.vn - Data Crowdsourcing Platform](https://www.facebook.com/dataset.vn)

### AIV Group - (+84) 931 458 189 - marketing@aivgroup.vn

AIV Group hướng đến việc ứng dụng những tiến bộ về công nghệ, đặc biệt là Trí tuệ nhân tạo (AI), Điện toán đám mây (Cloud Computing), Dữ liệu lớn (Big Data) để số hoá, hiện đại hoá các quy trình sản xuất và tiêu thụ thông tin đã tồn tại lâu đời trong xã hội Việt Nam, đồng thời góp phần giải quyết những vấn đề mới phát sinh trong lĩnh vực truyền thông do mặt trái của công nghệ như: vấn nạn tin giả, hình ảnh, video được cắt ghép tự động…

Website: [AIV Group](https://aivgroup.vn/)

Facebook: [AIV Group](https://www.facebook.com/aivgroup.jsc/)




# DANeS - Open-source E-newspaper dataset
![12613](https://user-images.githubusercontent.com/94349957/143620522-2b417ece-2482-4475-a261-120af096cb0d.jpg)
*<sub>Source: <a href="https://www.freepik.com/vectors/technology">Technology vector created by macrovector - www.freepik.com</a>.</sub>*


DANeS is an open-source E-newspaper dataset by collaboration between DATASET .JSC (dataset.vn) and AIV Group (aivgroup.vn) that contains over 600.000 online papers’ articles. The articles are gathered from a number of Vietnamese Publishing Houses such as: tuoitre.vn, baobinhduong.vn, baoquangbinh.vn, kinhtechungkhoan.vn, doanhnghiep.vn, vnexpress.net, ...

We hope to support the community by providing a multi-purpose set of raw data for different subjects (students, developers, companies, …). So if you create something with this dataset, please share with us through our e-mail: info@dataset.vn



<!-- TABLE OF CONTENTS -->
<details>
  <summary>Table of Contents</summary>
  <ol>
    <li><a href="#data-format">Data format</a>
    <li><a href="#labeling-process">Labeling process</a></li>
    <li><a href="#reviewing-process">Reviewing process</a></li>
    <li><a href="#updating-process">Updating process</a></li>
    <li><a href="#license-of-annotated-dataset">License of annotated dataset</a></li>
    <li><a href="#contact">Contact</a></li>
  </ol>
</details>

## Data format
The dataset is stored in [`.json`](https://www.json.org) format and has been divided into multiple batches. Here’s the example of each record's format:

| Key          | Type                   | Description                                  |
| ------------ | -----------------------| -------------------------------------------- |
| title        | string                 | title of the digital news                    |
| url          | string                 | link to the digital news                     |
| description  | string                 | description of the digital news              |

Example for a record of dataset:
```javascript
{
        	"text": "Ba ra đi vào ngày nhận điểm thi, nữ sinh được hỗ trợ học phí",
        	"meta": {
            			"description": "Ngày nhận được tin đỗ đại học cũng là lúc bố mất vì Covid-19, L.A dường như gục ngã. Thế nhưng, bên cạnh em đã có các mạnh thường quân hỏi han, hỗ trợ về kinh tế.",
            			"uri": "https://yan.vn/ba-ra-di-vao-ngay-nhan-diem-thi-nu-sinh-duoc-ho-tro-hoc-phi-277328.html"
        		}
    	}
``` 
 
## Labeling process

- Log in:
![DANeS 1 (1)](https://user-images.githubusercontent.com/94349957/144125798-d2ae5738-df36-4ca2-a1a3-778fd7dd5dd7.gif)


- The article should be classified under one out of three sentiment: Negative, Positive and Neutral. 
	
- The article will then be classified by topics: (em đính các topics sau ạ). Each article can carry numerous relevant and suitable topics. 

## Reviewing process

- 20% of the annotated records will be assigned to different annotator to re-annotate and compare with the original version.

- Quality Assurance members will randomly check 20-50% of the total annotated records.

## Updating process

- The raw data is expected to be fully uploaded at one time.

- The annotated records are expected to be updated once a month to official repository of DANeS (https://github.com/dataset-vn/DANeS)


## License of annotated dataset

<a rel="license" href="http://creativecommons.org/licenses/by/4.0/"><img alt="Giấy phép Creative Commons " style="border-width:0" src="https://i.creativecommons.org/l/by/4.0/88x31.png" /></a><br />
The annotated dataset of DANeS is licensed under Creative Commons Attribution 4.0 International License.

## About us

### DATASET.JSC - (+84) 98 442 0826 - info@dataset.vn

Website: [Dataset.vn](http://dataset.vn)

LinkedIn: [Dataset.vn - Data Crowdsourcing Platform](https://www.linkedin.com/company/dataset-vn/about/)

Facebook: [Dataset.vn - Data Crowdsourcing Platform](https://www.facebook.com/dataset.vn)

### AIV Group - (+84) 931 458 189 - marketing@aivgroup.vn

Website: [AIV Group](https://aivgroup.vn/)

Facebook: [AIV Group](https://www.facebook.com/aivgroup.jsc/)

