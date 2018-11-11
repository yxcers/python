import requests
from lxml import etree
import json


class TecentRecurit:
    def __init__(self):
        self.base_url = 'https://hr.tencent.com/position.php?&start={}#a'
        self.base_domain = 'https://hr.tencent.com/'
        self.headers = {
            'User-Agent':'Mozilla/5.0 (Windows NT 10.0; Win64; x64) '
                         'AppleWebKit/537.36 (KHTML, like Gecko) '
                         'Chrome/69.0.3497.100 Safari/537.36'
        }

    def get_url(self):
        url_lists = []
        for i in range(283):
            url = self.base_url.format(i * 10)
            url_lists.append(url)
        return url_lists

    def parse_url(self, url):
        position_url_lists = []
        response = requests.get(url,headers=self.headers)
        text = response.text
        html = etree.HTML(text)
        results = html.xpath('//table[@class="tablelist"]//td/a/@href')
        for result in results:
            position_url_lists.append(self.base_domain+result)
        return position_url_lists

    def parse_position_url(self,url):
        response = requests.get(url,headers=self.headers)
        text = response.content.decode()
        html = etree.HTML(text)
        position_information = {}
        position = html.xpath('//tr[@class="h"]//text()')[1]
        position_information['position'] = position.strip()
        tds = html.xpath('//tr[@class="c bottomline"]/td')
        address = tds[0].xpath('.//text()')[1]
        position_information['address'] = address
        category = tds[1].xpath('.//text()')[1]
        position_information['category'] = category
        nums = tds[2].xpath('.//text()')[1]
        position_information['nums'] = nums
        infos = html.xpath('//ul[@class="squareli"]')
        duty = infos[0].xpath('.//text()')
        position_information['duty'] = duty
        demand = infos[1].xpath('.//text()')
        position_information['demand'] = demand
        return position_information

    def save_position(self,dict):
        with open('腾讯招聘信息.txt','a',encoding='utf8') as f:
            f.write(json.dumps(dict,ensure_ascii=False)+'\n')

    def run(self):
        '''
        主程序
        '''
        # 1.获取社会招聘url
        url_lists = self.get_url()
        for url in url_lists:
            # 2.获取职位信息的url
            position_url_lists = self.parse_url(url)
            # 3.提取职位信息
            for position_url in position_url_lists:
                positon = self.parse_position_url(position_url)
            # 4.保存职位信息
                self.save_position(positon)


if __name__ == '__main__':
    tengxun = TecentRecurit()
    tengxun.run()
