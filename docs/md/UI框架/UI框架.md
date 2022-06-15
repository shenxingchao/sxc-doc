# UI框架
# Element ui
## 日期选择器的各种快捷选项 
<p align="left" style="color:#777777;">发布日期：2021-03-23</p>

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <link href="https://cdn.bootcss.com/element-ui/2.12.0/theme-chalk/index.css" rel="stylesheet">

    <script src="https://cdn.bootcss.com/vue/2.6.10/vue.min.js"></script>
    <script src="https://cdn.bootcss.com/element-ui/2.12.0/index.js"></script>
</head>
<body>
    <div id="app">
        <el-date-picker v-model="dateValue" size="small"
                        type="daterange"
                        value-format="yyyy-MM-dd"
                        range-separator="至"
                        start-placeholder="开始日期"
                        end-placeholder="结束日期"
                        v-bind:picker-options="pickerOptions"
                        >
        </el-date-picker>
    </div>
    <script>
        new Vue({
            el: '#app',
            data () {
                return {
                    dateValue: [],
                    pickerOptions: {
                        shortcuts: [{
                            text: '昨天',
                            onClick(picker) {
                                const end = new Date();
                                const start = new Date();
                                start.setTime(start.getTime() - 3600 * 1000 * 24);
                                end.setTime(end.getTime() - 3600 * 1000 * 24);
                                picker.$emit('pick', [start, end]);
                            }
                        }, {
                            text: '今天',
                            onClick(picker) {
                                const end = new Date();
                                const start = new Date();
                                picker.$emit('pick', [start, end]);
                            }
                        }, {
                            text: '上周',
                            onClick(picker) {
                                const oDate = new Date();
                                oDate.setTime(oDate.getTime() - 3600 * 1000 * 24 * 7);

                                var day = oDate.getDay()

                                var start = new Date(),
                                    end = new Date();
                                if (day == 0) {
                                    start.setDate(oDate.getDate());
                                    end.setDate(oDate.getDate() + 6);
                                } else {
                                    start.setTime(oDate.getTime() - 3600 * 1000 * 24 * day);
                                    end.setTime(oDate.getTime() + 3600 * 1000 * 24 * (6 - day));
                                }

                                picker.$emit('pick', [start, end]);
                            }
                        }, {
                            text: '本周',
                            onClick(picker) {
                                const end = new Date();
                                const start = new Date();
                                var thisDay = start.getDay();
                                var thisDate = start.getDate();
                                if (thisDay != 0) {
                                    start.setDate(thisDate - thisDay);
                                }
                                picker.$emit('pick', [start, end]);
                            }
                        }, {
                            text: '上月',
                            onClick(picker) {
                                const oDate = new Date();
                                var year = oDate.getFullYear();
                                var month = oDate.getMonth();
                                var start, end;
                                if (month == 0) {
                                    year--
                                    start = new Date(year, 11, 1)
                                    end = new Date(year, 11, 31)
                                } else {
                                    start = new Date(year, month - 1, 1)
                                    end = new Date(year, month, 0);
                                }

                                picker.$emit('pick', [start, end]);
                            }
                        }, {
                            text: '本月',
                            onClick(picker) {
                                const end = new Date();
                                const start = new Date();
                                start.setDate(1);
                                picker.$emit('pick', [start, end]);
                            }
                        }, {
                            text: '上季度',
                            onClick(picker) {
                                var oDate = new Date();

                                var thisYear = oDate.getFullYear();
                                var thisMonth = oDate.getMonth() + 1;

                                var n = Math.ceil(thisMonth / 3); // 季度，上一个季度则-1
                                var prevN = n - 1;
                                if (n == 1) {
                                    thisYear--
                                    prevN = 4;
                                }

                                var Month = prevN * 3; // 月份

                                var start = new Date(thisYear, Month - 3, 1);
                                var end = new Date(thisYear, Month, 0);

                                picker.$emit('pick', [start, end]);
                            }
                        }, {
                            text: '本季度',
                            onClick(picker) {
                                var oDate = new Date();

                                var thisYear = oDate.getFullYear();
                                var thisMonth = oDate.getMonth() + 1;

                                var n = Math.ceil(thisMonth / 3); // 季度

                                var Month = n * 3 - 1;

                                var start = new Date(thisYear, Month - 2, 1);
                                var end = new Date();

                                picker.$emit('pick', [start, end]);
                            }
                        }, {
                            text: '去年',
                            onClick(picker) {

                                var oDate = new Date();
                                var prevYear = oDate.getFullYear() - 1;
                                const end = new Date();
                                const start = new Date();
                                start.setFullYear(prevYear);
                                start.setMonth(0);
                                start.setDate(1);

                                end.setFullYear(prevYear);
                                end.setMonth(11);
                                end.setDate(31);

                                picker.$emit('pick', [start, end]);
                            }
                        }, {
                            text: '今年',
                            onClick(picker) {
                                const end = new Date();
                                const start = new Date();
                                start.setMonth(0);
                                start.setDate(1);
                                picker.$emit('pick', [start, end]);
                            }
                        }, {
                            text: '过往7天',
                            onClick(picker) {
                                const end = new Date();
                                const start = new Date();
                                start.setTime(start.getTime() - 3600 * 1000 * 24 * 7);
                                picker.$emit('pick', [start, end]);
                            }
                        }]
                    }
                }
            }
        })
    </script>
</body>
</html>
```

# Vant ui

# uview