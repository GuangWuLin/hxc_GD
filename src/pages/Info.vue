<template>
    <section class="info">
        <div class="info-inner">
            <h1>您知道您的Internet Explorer过时了吗？</h1>
            <p>为了得到我们最好的服务及体验效果，我们建议您升级到最新版本的Internet Explorer或选择另一个Web浏览器</p>
            <ul>
                <li>
                    <a href="http://go.microsoft.com/fwlink/?LinkId=324629">
                        <i class="glyph-icon icon-ie-broswer ie"></i>
                        <em>Internet Explorer 11</em>
                    </a>
                </li>
                <li>
                    <a href="http://download.ie.sogou.com/se/sogou_explorer_7.0_0502.exe">
                        <i class="glyph-icon icon-sougou-broswer sougou"></i>
                        <em>SouGou</em>
                    </a>
                </li>
                <li>
                    <a href="http://down.360safe.com/cse/360cse_8.7.0.306.exe">
                        <i class="glyph-icon icon-360-broswer qihu"></i>
                        <em>360</em>
                    </a>
                </li>
            </ul>
        </div>
    </section>
</template>

<style lang="scss">
.info {
    position: absolute;
    top: 50%;
    left: 50%;
    width: 600px;
    height: 300px;
    background: #fff;
    margin: -150px 0 0 -301px;
    border-radius: .5em;
    -moz-box-shadow: 0 0 20px #FF4949;
    -webkit-box-shadow: 0 0 20px #FF4949;
    box-shadow: 0 0 20px #FF4949;
    .info-inner {
        margin: 50px;
        h1 {
            font-size: 1.8em;
        }
        p {
            margin-top: 10px;
            font-size: 1.2em;
            margin-bottom: 20px;
        }
        ul {
            li {
                display: inline-block;
                width: 32%;
                height: 100px;
                padding: 0;
                margin: 0;
                list-style: none;
                a {
                    display: block;
                    width: 100%;
                    height: 100%;
                    em {
                        font-size: .6em;
                        text-align: center;
                        display: block;
                        position: relative;
                        top: 8px;
                        color: #999;
                        font-style: normal;
                    }
                }
            }
        }
        .glyph-icon {
            margin-right: 0;
            float: none;
            font-size: 4em;
            margin-top: 10px;
            &.ie {
                color: #58B7FF;
            }
            &.sougou {
                color: #20A0FF;
                font-size: 4.5em；
            }
            &.qihu {
                color: #13CE66;
            }
        }
    }
}
</style>