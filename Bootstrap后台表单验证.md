

### 前言
做Web开发的我们，表单验证是再常见不过的需求了。友好的错误提示能增加用户体验。博主搜索bootstrap表单验证，搜到的结果大部分都是文中的主题：bootstrapvalidator。今天就来看看它如何使用吧。

### 源码及API地址

介绍它之前，还是给出它的源码的地址吧

[bootstrapvalidator源码](https://github.com/nghuuphuoc/bootstrapvalidator)



    <link rel="stylesheet" href="../vendor/bootstrap/css/bootstrap.css"/>
    <link rel="stylesheet" href="../dist/css/bootstrapValidator.css"/>
    <script type="text/javascript" src="../vendor/jquery/jquery.min.js"></script>
    <script type="text/javascript" src="../vendor/bootstrap/js/bootstrap.min.js"></script>
    <script type="text/javascript" src="../dist/js/bootstrapValidator.js"></script>

html代码
```html
        <form id="createModal"  action="{{route('goods.update')}}" method="post" id="defaultForm">
            {{csrf_field()}}
            <input type="hidden" name="id" value="">
            <div class="form-group">
                <label class=" control-label">商品名称</label>
                <div class="">
                    <input type="text" class="form-control" name="name" />
                </div>
            </div>
            <div class="form-group">
                <label class=" control-label">商品价格(元/个)</label>
                <div class="">
                    <input type="text" class="form-control" name="price" />
                </div>
            </div>
            <div class="modal-footer">
                <button type="button" class="btn btn-default" data-dismiss="modal">Close</button>
                <button type="submit" class="btn btn-primary">Yes</button>
            </div>
        </form>

```

javacript

```javascript

$('#createModal').bootstrapValidator({
    //        live: 'disabled',
    message: 'This value is not valid',
    feedbackIcons: {
        valid: 'glyphicon glyphicon-ok',
        invalid: 'glyphicon glyphicon-remove',
        validating: 'glyphicon glyphicon-refresh'
    },
    fields: {
        name: {
            message: 'The username is not valid',
            validators: {
                notEmpty: {
                    message: '商品名是必需的，不能为空'
                },
            }
        },
        price: {
            validators: {
                notEmpty: {
                    message: '价格是必需的，不能为空'
                },
                regexp: {
                    regexp: /^[0-9\.]+$/,
                    message: '价格只能由数字组成'
                },
            }
        }
    }
})

```

bootstrapvalidator 上手很简单，作者给出了丰富的 `demo` 可以在 `GitHub` 中查看 `demo` 源码。
