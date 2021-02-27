---
title: java后台模拟浏览器发送请求
date: 2019-08-12 10:21:28
tags:
- java
- http请求
categories:
- java
---
1.URLConnection方式
> 导入jar包

	import org.springframework.web.multipart.MultipartFile;
	import java.io.*;
	import java.net.HttpURLConnection;
	import java.net.URL;
	import java.net.URLConnection;
	import java.security.PrivateKey;
	import java.security.PublicKey;
	import java.util.*;

> 请求工具类

	public class HttpUtil {
    private final static String BOUNDARY = UUID.randomUUID().toString();// 边界标识
    private final static String PREFIX = "--";// 必须存在
    private final static String LINE_END = "\r\n";

    /**
     * 向指定URL发送GET方法的请求
     *
     * @param url   发送请求的URL
     * @param param 请求参数，请求参数应该是 name1=value1&name2=value2 的形式。
     * @return URL 所代表远程资源的响应结果
     */
    public static String sendGet(String url, String param) {
        String result = "";
        BufferedReader in = null;
        try {
            String urlNameString = url + "?" + param;
            URL realUrl = new URL(urlNameString);
            // 打开和URL之间的连接
            URLConnection connection = realUrl.openConnection();
            // 设置通用的请求属性
            connection.setRequestProperty("accept", "*/*");
            connection.setRequestProperty("connection", "Keep-Alive");
            connection.setRequestProperty("user-agent",
                    "Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1;SV1)");
            /*设置超时时间*/
            connection.setConnectTimeout(0);
            connection.setReadTimeout(0);

            // 建立实际的连接
            connection.connect();
            // 获取所有响应头字段
            Map<String, List<String>> map = connection.getHeaderFields();
            // 遍历所有的响应头字段
            for (String key : map.keySet()) {
                System.out.println(key + "--->" + map.get(key));
            }
            // 定义 BufferedReader输入流来读取URL的响应
            in = new BufferedReader(new InputStreamReader(
                    connection.getInputStream()));
            String line;
            while ((line = in.readLine()) != null) {
                result += line;
            }
        } catch (Exception e) {
            System.out.println("发送GET请求出现异常！" + e);
            e.printStackTrace();
        }
        // 使用finally块来关闭输入流
        finally {
            try {
                if (in != null) {
                    in.close();
                }
            } catch (Exception e2) {
                e2.printStackTrace();
            }
        }
        return result;
    }

    /**
     * 向指定 URL 发送POST方法的请求
     *
     * @param url   发送请求的 URL
     * @param param 请求参数，请求参数应该是 name1=value1&name2=value2 的形式。
     * @return 所代表远程资源的响应结果
     */
    public static String sendPost(String url, String param) {
        PrintWriter out = null;
        BufferedReader in = null;
        String result = "";
        try {
            URL realUrl = new URL(url);
            // 打开和URL之间的连接
            HttpURLConnection conn = (HttpURLConnection) realUrl.openConnection();
            // 设置通用的请求属性
            conn.setRequestProperty("accept", "*/*");
            conn.setRequestProperty("connection", "Keep-Alive");
            conn.setRequestProperty("user-agent",
                    "Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1;SV1)");
            // 发送POST请求必须设置如下两行
            conn.setDoOutput(true);
            conn.setDoInput(true);
            /*设置超时时间*/
            conn.setConnectTimeout(0);
            conn.setReadTimeout(0);
            // 获取URLConnection对象对应的输出流
            out = new PrintWriter(conn.getOutputStream());
            // 发送请求参数
            out.print(param);
            // flush输出流的缓冲
            out.flush();
            int responseCode = conn.getResponseCode();
            System.out.println("返回状态" + responseCode);
            // 定义BufferedReader输入流来读取URL的响应
            in = new BufferedReader(
                    new InputStreamReader(conn.getInputStream()));
            String line;
            while ((line = in.readLine()) != null) {
                result += line;
            }
        } catch (Exception e) {
            System.out.println("发送 POST 请求出现异常！" + e);
            e.printStackTrace();
        }
        //使用finally块来关闭输出流、输入流
        finally {
            try {
                if (out != null) {
                    out.close();
                }
                if (in != null) {
                    in.close();
                }
            } catch (IOException ex) {
                ex.printStackTrace();
            }
        }
        return result;
    }

    /**
     * 向指定 URL 发送POST方法的请求(带文件)
     *
     * @param url   发送请求的 URL
     * @param paramMap参数map requestFile 文件参数Map。
     * @return 所代表远程资源的响应结果
     */
    public static String sendPostFile(String url, Map<String, String> paramMap, Map<String, MultipartFile> requestFile) {
        DataOutputStream out = null;
        BufferedReader in = null;
        String result = "";
        try {
            URL realUrl = new URL(url);
            // 打开和URL之间的连接
            HttpURLConnection conn = (HttpURLConnection) realUrl.openConnection();
            // 设置通用的请求属性
            conn.setRequestProperty("accept", "*/*");
            conn.setRequestProperty("connection", "Keep-Alive");
            conn.setRequestProperty("user-agent",
                    "Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1;SV1)");
            conn.setRequestProperty("Content-Type", "multipart/form-data;boundary=" + BOUNDARY);
            // 发送POST请求必须设置如下两行
            conn.setDoOutput(true);
            conn.setDoInput(true);
            /*设置超时时间*/
            conn.setConnectTimeout(0);
            conn.setReadTimeout(0);
            // 获取URLConnection对象对应的输出流
            out = new DataOutputStream(conn.getOutputStream());
            // 发送请求参数
            writeParams(paramMap, out);
            // flush输出流的缓冲
            out.flush();
            writeFile(requestFile, out);
            int responseCode = conn.getResponseCode();
            System.out.println("返回状态" + responseCode);
            // 定义BufferedReader输入流来读取URL的响应
            in = new BufferedReader(
                    new InputStreamReader(conn.getInputStream()));
            String line;
            while ((line = in.readLine()) != null) {
                result += line;
            }
        } catch (Exception e) {
            System.out.println("发送 POST 请求出现异常！" + e);
            e.printStackTrace();
        }
        //使用finally块来关闭输出流、输入流
        finally {
            try {
                if (out != null) {
                    out.close();
                }
                if (in != null) {
                    in.close();
                }
            } catch (IOException ex) {
                ex.printStackTrace();
            }
        }
        return result;
    }

    /**
     * 对post参数进行编码处理并写入数据流中
     *
     * @throws Exception
     * @throws IOException
     */
    private static void writeParams(Map<String, String> requestText,
                                    OutputStream os) throws Exception {
        try {
            String msg = "请求参数部分:\n";
            if (requestText == null || requestText.isEmpty()) {
                msg += "空";
            } else {
                StringBuilder requestParams = new StringBuilder();
                Set<Map.Entry<String, String>> set = requestText.entrySet();
                Iterator<Map.Entry<String, String>> it = set.iterator();
                while (it.hasNext()) {
                    Map.Entry<String, String> entry = it.next();
                    requestParams.append(PREFIX).append(BOUNDARY).append(LINE_END);
                    requestParams.append("Content-Disposition: form-data; name=\"")
                            .append(entry.getKey()).append("\"").append(LINE_END);
                    requestParams.append("Content-Type: text/plain; charset=utf-8")
                            .append(LINE_END);
                    requestParams.append("Content-Transfer-Encoding: 8bit").append(
                            LINE_END);
                    requestParams.append(LINE_END);// 参数头设置完以后需要两个换行，然后才是参数内容
                    requestParams.append(entry.getValue());
                    requestParams.append(LINE_END);
                }
                os.write(requestParams.toString().getBytes());
                os.flush();

                msg += requestParams.toString();
            }
            System.out.println(msg);
        } catch (Exception e) {
            e.printStackTrace();
            throw new Exception(e);
        }
    }

    /**
     * 对post上传的文件进行编码处理并写入数据流中
     *
     * @throws IOException
     */
    private static void writeFile(Map<String, MultipartFile> requestFile,
                                  DataOutputStream out) throws Exception {
        //文件上传
        StringBuilder fileSb = new StringBuilder();
        for (Map.Entry<String, MultipartFile> fileEntry : requestFile.entrySet()) {
            fileSb.append(PREFIX)
                    .append(BOUNDARY)
                    .append(LINE_END)
                    /**
                     * 这里重点注意： name里面的值为服务端需要的key 只有这个key 才可以得到对应的文件
                     * filename是文件的名字，包含后缀名的 比如:abc.png
                     */
                    .append("Content-Disposition: form-data; name=\"file\"; filename=\""
                            + fileEntry.getKey() + "\"" + LINE_END)
                    .append("Content-Type: image/jpg" + LINE_END) //此处的ContentType不同于 请求头 中Content-Type
                    .append("Content-Transfer-Encoding: 8bit" + LINE_END)
                    .append(LINE_END);// 参数头设置完以后需要两个换行，然后才是参数内容
            out.writeBytes(fileSb.toString());
            out.flush();
            InputStream is = fileEntry.getValue().getInputStream();
            byte[] buffer = new byte[1024];
            int len = 0;
            while ((len = is.read(buffer)) != -1) {
                out.write(buffer, 0, len);
            }
            is.close();
            out.writeBytes(LINE_END);
        }
        //请求结束标志
        out.writeBytes(PREFIX + BOUNDARY + PREFIX + LINE_END);
        out.flush();
    }

    public static void main(String[] args) {
        String s = sendPost("", "");
        System.out.println(s);
    }
	}
