<pre>
{security, [
    %% verify_none:不验证客户端身份;verify_peer:验证客户端身份
    {verify, verify_peer},

    %% 服务器证书文件
    {server_cert_file, "/mnt/hgfs/work/com.hx.ewp/ewp5.2/public/security/test/ServerCert.crt"},

    %% 服务器私钥文件
    {server_key_file, "/mnt/hgfs/work/com.hx.ewp/ewp5.2/public/security/test/ServerKey.pem"},

    %% 服务器私钥密码
    {server_key_pwd, "12345678"},

    %% ca证书文件
    {ca_cert_file, "/mnt/hgfs/work/com.hx.ewp/ewp5.2/public/security/test/CACert.crt"},

    %% 证书颁发者私钥文件
    {issuer_key_file, "/mnt/hgfs/work/com.hx.ewp/ewp5.2/public/security/test/CAKey.pem"},

    %% 证书颁发者私钥密码
    {issuer_key_pwd, "12345678"},

    %% 支持最老的客户端协议版本 {Major,Minor}
    {oldest_supported_ver, {1, 0}},

     %% 客户端防篡改配置
    {client_verify,[
                    %% 客户端防篡改校验开关标识
                    {flag,false},
                    %% 客户端防篡改校验文件签名文件目录
                    {client_verify_file,"/mnt/hgfs/work/com.hx.ewp/ewp5.2/public/security/client_file"},
                    %%需要校验的信道版本号
                    {verify_version,[{1,3}]},
                    %%不需要校验的信道版本号
                    {pass_version,[{1, 0},{1, 2}]},
                    %%不需要校验的平台号例如{pass_platform,[iphone","ipad"]},
                    {pass_platform,[]},
                    %%校验失败提示模板名
                    {error,"ewp_verify_fail"}
                    ]}
]}.
</pre>