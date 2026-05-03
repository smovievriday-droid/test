conftest.py

import pytest
from selenium import webdriver


def pytest_addoption(parser):
    parser.addoption(
        "--language",
        action="store",
        default="en",
        help="Choose language: en, ru, es, fr, etc."
    )


@pytest.fixture(scope="function")
def browser(request):
    user_language = request.config.getoption("language")

    options = webdriver.ChromeOptions()
    options.add_experimental_option(
        "prefs",
        {"intl.accept_languages": user_language}
    )

    browser = webdriver.Chrome(options=options)
    browser.implicitly_wait(5)

    yield browser

    browser.quit()

test_items.py

import time
from selenium.webdriver.common.by import By


def test_guest_should_see_add_to_basket_button(browser):
    link = "http://selenium1py.pythonanywhere.com/catalogue/coders-at-work_207/"
    browser.get(link)

    # Пауза нужна для визуальной проверки языка интерфейса
    time.sleep(30)

    add_to_basket_button = browser.find_elements(
        By.CSS_SELECTOR,
        "button.btn-add-to-basket"
    )

    assert len(add_to_basket_button) > 0, "Add to basket button is not found"
