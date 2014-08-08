project_demo
============
require "json"
require "selenium-webdriver"
require 'webdriver-user-agent'
gem "test-unit"
require "test/unit"

class Rb04Chrome < Test::Unit::TestCase

  def setup
    @driver = Selenium::WebDriver.for :chrome, :switches => %W[--user-agent=#{'Mozilla/5.0 (iPhone; CPU iPhone OS 5_0 like Mac OS X) AppleWebKit/534.46 (KHTML, like Gecko) Version/5.1 Mobile/9A334 Safari/7534.48.3'}]
    #@driver.goto 'http://ny:5738@sptest.ny-onlinestore.com'
    #@base_url = "https://sptest.ny-onlinestore.com/"
    @base_url = "http://ny:5738@sptest.ny-onlinestore.com"
    @accept_next_alert = true
    @driver.manage.timeouts.implicit_wait = 30
    @verification_errors = []
  end
  
  def teardown
    @driver.quit
    assert_equal [], @verification_errors
  end
  
  def test_rb04_chrome
    #@driver.get(@base_url)
    @driver.navigate.to("http://ny:5738@sptest.ny-onlinestore.com")

    @driver.find_element(:link, "会員登録").click
    @driver.find_element(:name, "name").clear
    @driver.find_element(:name, "name").send_keys "mti1001"
    @driver.find_element(:name, "email").clear
    @driver.find_element(:name, "email").send_keys "nnbinh0301@gmail.com"
    @driver.find_element(:name, "email2").clear
    @driver.find_element(:name, "email2").send_keys "nnbinh0301@gmail.com"
    @driver.find_element(:name, "password").clear
    @driver.find_element(:name, "password").send_keys "zaq12wsx"
    @driver.find_element(:name, "password2").clear
    @driver.find_element(:name, "password2").send_keys "zaq12wsx"
    @driver.find_element(:id, "e4_r_1").click
    @driver.find_element(:name, "birth_year").clear
    @driver.find_element(:name, "birth_year").send_keys "1991"
    @driver.find_element(:name, "birth_month").clear
    @driver.find_element(:name, "birth_month").send_keys "01"
    @driver.find_element(:name, "birth_day").clear
    @driver.find_element(:name, "birth_day").send_keys "03"
    @driver.find_element(:id, "login_keep").click
    @driver.find_element(:id, "entry_submit_w").click
  end
  
  def element_present?(how, what)
    @driver.find_element(how, what)
    true
  rescue Selenium::WebDriver::Error::NoSuchElementError
    false
  end
  
  def alert_present?()
    @driver.switch_to.alert
    true
  rescue Selenium::WebDriver::Error::NoAlertPresentError
    false
  end
  
  def verify(&blk)
    yield
  rescue Test::Unit::AssertionFailedError => ex
    @verification_errors << ex
  end
  
  def close_alert_and_get_its_text(how, what)
    alert = @driver.switch_to().alert()
    alert_text = alert.text
    if (@accept_next_alert) then
      alert.accept()
    else
      alert.dismiss()
    end
    alert_text
  ensure
    @accept_next_alert = true
  end
end
