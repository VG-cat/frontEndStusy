```
import { saveAs } from "file-saver";
const XLSX = require('xlsx');
```







## 导出，table选择某几列后

```
    const handleExportCurrentExcel = () => {
        const temp = columns.map(item => item.title)
        temp.splice(-1, 1)    //去除最后一列，操作列
        
        const subTemp = new Array(columns.length - 1).fill('');
        const majorHeader = [[...temp], [...subTemp, '心理测评年份', '心理测评结果', '自杀倾向']]   //表头数据，两行
        console.log(majorHeader);


        const majorData: any[] = [];
        selectedRows.forEach((values) => {   //selectedRows选择的列表对象数据
            majorData.push({       			//按照顺序处理
                // id: d.id,
                stuId: values.stuId,
                stuName: values.stuName,
                stuClass: values.stuClass,
                college: values.college,
                contact: values.contact,
                birthDate: moment(values.birthDate).format('YYYY-MM-DD'),
                studyCompletionRate: values.studyCompletionRate,

                supportRate: values.supportRate,
                rankingPercentage: values.rankingPercentage,
                sportsScore: values.sportsScore,

                volunteerHours: values.volunteerHours,
                academicTechnology: values.academicTechnology,
                socialWork: values.socialWork,

                sportsQuality: values.sportsQuality,
                culturalArts: values.culturalArts===1？"是"：”否“,

            });

        });
        
        
        
        const majorWorksheet = XLSX.utils.aoa_to_sheet(majorHeader);  //创建sheet
       	majorWorksheet["!merges"] = [       //表头合并，s开始位置，e结束位置，r行，c列
          { s: { r: 0, c: 0 }, e: { r: 1, c: 0 } },
          { s: { r: 0, c: 1 }, e: { r: 1, c: 1 } },
          { s: { r: 0, c: 2 }, e: { r: 1, c: 2 } },
          { s: { r: 0, c: 3 }, e: { r: 1, c: 3 } },
          { s: { r: 0, c: 4 }, e: { r: 1, c: 4 } },
          { s: { r: 0, c: 5 }, e: { r: 1, c: 5 } },
          { s: { r: 0, c: 6 }, e: { r: 1, c: 6 } },
          { s: { r: 0, c: 7 }, e: { r: 1, c: 7 } },
          { s: { r: 0, c: 8 }, e: { r: 0, c: 11 } },
        ];
    
        XLSX.utils.sheet_add_json(majorWorksheet, majorData, { origin: 'A2', skipHeader: true });  //从A2开始添加数据
        const workbook = XLSX.utils.book_new();
        XLSX.utils.book_append_sheet(workbook, majorWorksheet, '团员发展信息');



        const wbout = XLSX.write(workbook, { bookType: 'xlsx', type: 'binary' });

        function s2ab(s) {
            const buf = new ArrayBuffer(s.length);
            const view = new Uint8Array(buf);
            for (let i = 0; i !== s.length; ++i) view[i] = s.charCodeAt(i) & 0xFF;
            return buf;
        }

        saveAs(new Blob([s2ab(wbout)], { type: 'application/octet-stream' }), '团员发展信息.xlsx');
    }
```

## 导入为excel

```
    const handleUpload = (file) => {
        const reader = new FileReader();
        const yesOrNo = {
            '是': '1',
            '否': '0',
        };
        const genderMap = {
            '男': '1',
            '女': '0',
        };

        reader.onload = (e) => {
            console.log("File loaded successfully");

            const contents = e.target.result;
            const workbook = XLSX.read(contents, { type: 'binary' });
            const worksheet = workbook.Sheets[workbook.SheetNames[0]];  //选择工作簿中的第一个工作表
            const jsonData = XLSX.utils.sheet_to_json(worksheet, { header: 2 }).slice(2);
//头两行为表头，并去除
            console.log("Parsed JSON data:", jsonData);
            const temp = columns.map(item => item.key)
            temp.splice(-1, 1)
            const headerRow = temp;
            
            const formattedData = jsonData.map((row) => {
                const obj = {};

                headerRow.forEach((header, index) => {
                    if (header === 'gender') {
                        obj[header] = genderMap[row[index]];
                    } else if (header === 'isYouthLeagueMember' || header === 'isOutstandingHonors') {
                        obj[header] = yesOrNo[row[index]];
                    } else {
                        obj[header] = row[index];
                    }
                });
                // obj.stage = importType;
                return obj;
            });


            console.log("Formatted data:", formattedData);

			//将excel数据获取，可以传给后端
            insertListMemberDevelopmentInformation(formattedData).then((res) => {
                if (res.code === "200") {
                    // 显示提交成功的消息
                    message.success('导入成功');
                    // 关闭弹出框
                    queryMemberDevelopmentObjectbystuId({ roleLabel: '班级团支书', stuId: localStorage.getItem('userAccount') }).then((res) => {
                        if (res.code == 200) {
                            console.log(res.result);

                            setDataSource(res.result)
                            setInitialData(res.result)
                        }


                    });
                } else {
                    // 显示提交失败的消息
                    message.error('导入失败，请重试');
                }
            }
            );
        };

        reader.onerror = (e) => {
            console.error("Error reading file:", e);
        };

        reader.readAsBinaryString(file);
    };
```

