---
title: React Native控件的使用之ListView
date: 2017-03-18 20:28:05
tags: [React Native]
categories: 技术
---
##### 1.ListView控件
  功能：用于显示一个可垂直滚动的变化的数据列表。下面用一个简单的例子来展示一下ListView的用法
<!--more-->
###### 1.1 案例效果
![ListViewTest](http://oj056g1gy.bkt.clouddn.com/ListViewTest1.gif)
###### 1.2 案例代码
```
//定义原始数据：数组（字符串）
//假装一数组的男神都是我的>_<
    var contents =[
        '1. 韩庚', 
        '2. 韩庚',
        '3. 韩庚',
        '4. 韩庚',
        '5. 韩庚',
        '6. 韩庚',
        '7. 韩庚',
        '8. 韩庚',
        '9. 韩庚',
        '10. 韩庚',
        '11. 韩庚',
        '12. 韩庚',
        '13. 韩庚'
    ];

//ListViewTest1
export default class ListViewTest1 extends Component{
    constructor(props){
        super(props);
        //创建一个ListView.DataSource数据源，然后给它传递一个普通的数据数组
        let ds = new ListView.DataSource({
            //通过判断决定渲染哪些行组件，避免全部渲染，提高渲染效率
            rowHasChanged: (oldRow, newRow) => oldRow != newRow
        });
        this.state = {
            //在dataSource中，不直接使用提供的原始数据
            //使用cloneWithRows方法对数据源进行复制，
            //使用复制后的数据源实例化ListView
            //好处：当原始数据发生变化时，ListView的dataSource不会改变
            dataSource: ds.cloneWithRows(contents)
        }
    }
    //渲染row的方法，参数是每行显示的数据对象
    _renderRow(rowData:string){
        return (
            <View style={styles.row}>
                <Text style={styles.content}>{rowData}</Text>
            </View>
        )
    }
    render(){
        return (
            <ListView
                style={styles.container}
                //设置数据源
                dataSource={this.state.dataSource}
                //配置每一行的组件
                renderRow={this._renderRow}
            />
        )
    }
}

const styles = StyleSheet.create({
    container:{
        flex:1,
        marginTop:25
    },
    row:{
        justifyContent:'center',
        alignItems:'center',
        padding:25,
        borderBottomWidth:1,
        borderColor:"#ccc",
    },
    content:{
        flex:1,
        fontSize:20,
        color:'blue'
    }
});
```

##### 2.ScrollView控件

###### 2.1 案例演示

###### 2.2 案例代码

```
import React ,{Component} from 'react';
import{
    View,
    Text,
    RefreshControl,
    StyleSheet,
    ScrollView,
    TouchableWithoutFeedback
}from 'react-native';
class Row extends Component{
    _onClick(){
        this.props.onClick(this.props.data)
    };
    render(){
        return (
            <TouchableWithoutFeedback
                style={{backgroundColor:'red'}}
                onPress={this._onClick.bind(this)}
            >
                <View style={styles.row}>
                    <Text>
                        {this.props.data.text+' 已收到'+this.props.data.clicks+'颗心'}
                    </Text>
                </View>
            </TouchableWithoutFeedback>
        )
    }
}
export default class RefreshControlTest extends Component{
    constructor(props){
        super(props);
        this.state={
            isRefreshing:false,
            rowData:Array.from(new Array(20)).map(
                (val,i)=>({text:'韩庚韩庚我爱韩庚'+i,clicks:0})
            )
        }
    };
    _onClick(row){
        row.clicks++;
        this.setState({
            rowData: this.state.rowData,
        });
    };
    render(){
        const rows = this.state.rowData.map(
            (row,ii)=>{
                return (<Row key={ii} data={row} onClick={this._onClick.bind(this)} />)
            }
        );
        return(
            <ScrollView
                style={styles.containers}
                refreshControl={
                    <RefreshControl
                        refreshing={this.state.isRefreshing}
                        onRefresh={this._onRefresh.bind(this)}
                        colors={['#FFB3B3']}
                        progressBackgroundColor="#fff"
                    />
                }
            >
                {rows}
            </ScrollView>
        );
    }
    _onRefresh(){
        this.setState({isRefreshing:true});
        setTimeout(()=>{
            const rowData = this.state.rowData.map((item,i)=>(
                {text:'送你我的小心心 点击送心：',clicks:0}
            ));
            this.setState({
                isRefreshing:false,
                rowData:rowData
            })
        },2000)
    }
}

const styles = StyleSheet.create({
    containers:{
        flex:1,
        backgroundColor:"#eee"
    },
    row:{
        flex:1,
        padding:25,
        borderWidth:1,
        borderColor:'#000'
    }
});
```