---

layout: default
title: jQuery 望远镜数据表格
abstract: 
comments: false

---

## 望远镜数据表格

- 这是一个jQuery插件，用于显示望远镜数据.

```javascript

(function($){

var tableHtml = function(data){
    
    var meta = data.metaData;
    var fields = meta.fields;
    var data = data.data;
    var buf = ['<table bordercolorlight="#014E4B" bordercolordark="white"'
        + ' width="100%" border="1" align="center" cellpadding="0" cellspacing="0" >'];
    
    //表头
    if(fields && fields.length > 0){
        buf.push('<tr style="padding:2px;">');
        $.each(fields, function(i, f){
            if(f && f.descCh && f.hidden !== true){
                buf.push('<td class="titletd" style="text-align:center;height:20px;width:'
                + f.width + '%;">'
                + f.descCh + '</td>');
            }
        });
        buf.push('</tr>');
        
        //数据
        if(data){
            $.each(data, function(i, d){
                buf.push('<tr class="tr' + (i%2==0?'1':'2') 
                    + '" style="cursor: default;" onmouseover="MoveOn(this);" onmouseout="MoveOut(this);">');
                $.each(fields, function(i, f){
                    if(f && f.name && f.hidden !== true){
                        var v = d[f.name];
                        if(typeof v == 'string'){
                            v = $.trim(v) == '' ? '&nbsp' : v;
                        }else if(v == undefined){
                            v = '&nbsp;';
                        }
                        buf.push('<td style="padding:2px;height:20px;">'
                        + '<input type="text" class="fontright1" style="text-align:' + f.align + ';" readonly value=" ' + v + ' "/>'
                        + '</td>');
                    };
                });
                
                buf.push('</tr>');
            });
        }
    }
    
    buf.push('</table>');
    
    return buf.join('');
};

/**
 * code : 望远镜编码
 * condition : 查询条件
 * config : 配置
 * 
 config
 {
    limit : 100 //
    //sortField : 'ORDER_NO' 
 }

 */
var plugin = function(code, condition, config){
    
    var me = this;
    var limit = config && config.limit ? config.limit : 100;
    var sortField = config ? config.sortField : null;
    
    var params = 'params={"progCode":"' + code
        + '","progCondition":"' + condition + '",'
        //+ (sortField ? '"sortInfo":{"sort":"' + sortField + '","dir":"DESC"},' : '')
        + '"metaData":{"paramNames":{"start":"start","limit":"limit","sort":"sort","dir":"dir"},'
        + '"idProperty":"id","root":"data","totalProperty":"total","successProperty":"success","messageProperty":"message",'
        + '"limit":' + limit + ',"loadMetaData":true},"xaction":"read"}';
    $.ajax({
        type: "POST",
        url: '/rsc/rsclient/generalsel',
        data: params,
        headers : {
            'Rs-method' : 'read',
            'Rs-dataType':'json',
            'Rs-accept':'json'
        },
        success : function(data){
            me.html(tableHtml(data));
        },
        failure : function(){
            me.html('Î´»ñµÃÊý¾Ý');
        }
    });
};

$.fn.generalselGrid = plugin;

})(jQuery);

```