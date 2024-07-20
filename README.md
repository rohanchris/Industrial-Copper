# Industrial-Copper

import streamlit as st
import pickle
import numpy as np
import sklearn
from streamlit_option_menu import option_menu

Imports:
streamlit: Used for creating interactive web applications.
pickle: For loading pre-trained machine learning models.
numpy: For handling numerical data.
sklearn: Though not directly used here, typically used for machine learning models and processing.
streamlit_option_menu: For creating a sidebar navigation menu in the Streamlit app.

def predict_status(ctry, itmtp, aplcn, wth, prdrf, qtlg, cstlg, tknslg, slgplg, itmdt, itmmn, itmyr, deldtdy, deldtmn, deldtyr):
Function Definition: This line defines a function named predict_status. It takes multiple parameters representing various features required for prediction, such as country, item type, application, width, product reference, and others including date components.

    #change the datatypes "string" to "int"
    itdd = int(itmdt)
    itdm = int(itmmn)
    itdy = int(itmyr)
Convert Item Date Components: These lines convert the item date components (itmdt, itmmn, itmyr) from string format to integers. This is necessary because machine learning models typically require numerical inputs for processing.

    dydd = int(deldtdy)
    dydm = int(deldtmn)
    dydy = int(deldtyr)
Convert Delivery Date Components: Similar to the item date components, these lines convert the delivery date components (deldtdy, deldtmn, deldtyr) from string format to integers.

    #modelfile of the classification
    with open("C:/Users/dell/Industry Copper/Classification_model.pkl", "rb") as f:
        model_class = pickle.load(f)
Load Classification Model: This block of code opens a file that contains the pre-trained classification model (Classification_model.pkl) using the pickle module and loads the model into the variable model_class.

    user_data = np.array([[ctry, itmtp, aplcn, wth, prdrf, qtlg, cstlg, tknslg, slgplg, itdd, itdm, itdy, dydd, dydm, dydy]])
Prepare User Data: This line creates a NumPy array called user_data. The array is shaped as a 2D array with a single row (one sample) and multiple columns (features). Each feature is populated with the corresponding input parameter value. This structure is required for the model to process the data correctly.

    y_pred = model_class.predict(user_data)
Make Prediction: The pre-trained classification model (model_class) is used to predict the outcome based on the input user_data. The result of this prediction is stored in the variable y_pred. This is typically a binary value (0 or 1) in the context of a classification problem.

    if y_pred == 1:
        return 1
    else:
        return 0
Return Prediction Result: This conditional block checks the predicted value (y_pred). If y_pred equals 1, it returns 1, indicating a "won" status. If y_pred equals 0, it returns 0, indicating a "lose" status.

def predict_selling_price(ctry, sts, itmtp, aplcn, wth, prdrf, qtlg, cstlg,
                          tknslg, itmdt, itmmn, itmyr, deldtdy, deldtmn, deldtyr):
Function Definition: This function named predict_selling_price takes multiple parameters representing various features required for prediction, such as country, status, item type, application, width, product reference, and date components.

    #change the datatypes "string" to "int"
    itdd = int(itmdt)
    itdm = int(itmmn)
    itdy = int(itmyr)
Convert Item Date Components: These lines convert the item date components (itmdt, itmmn, itmyr) from string format to integers. This is necessary because machine learning models typically require numerical inputs for processing.

    dydd = int(deldtdy)
    dydm = int(deldtmn)
    dydy = int(deldtyr)
Convert Delivery Date Components: Similar to the item date components, these lines convert the delivery date components (deldtdy, deldtmn, deldtyr) from string format to integers.

    #modelfile of the classification
    with open("C:/Users/dell/Industry Copper/Regression_Model.pkl", "rb") as f:
        model_regg = pickle.load(f)
Load Regression Model: This block of code opens a file that contains the pre-trained regression model (Regression_Model.pkl) using the pickle module and loads the model into the variable model_regg.
Load Regression Model: This block of code opens a file that contains the pre-trained regression model (Regression_Model.pkl) using the pickle module and loads the model into the variable model_regg.

    user_data = np.array([[ctry, sts, itmtp, aplcn, wth, prdrf, qtlg, cstlg, tknslg,
                           itdd, itdm, itdy, dydd, dydm, dydy]])
Prepare User Data: This line creates a NumPy array called user_data. The array is shaped as a 2D array with a single row (one sample) and multiple columns (features). Each feature is populated with the corresponding input parameter value. This structure is required for the model to process the data correctly.

    y_pred = model_regg.predict(user_data)
Make Prediction: The pre-trained regression model (model_regg) is used to predict the selling price based on the input user_data. The result of this prediction is stored in the variable y_pred.

    ac_y_pred = np.exp(y_pred[0])
Convert Prediction: The predicted value (y_pred) is exponentiated using the natural exponential function (np.exp). This step is necessary if the model's prediction is in the log scale, converting it back to the original scale.

    return ac_y_pred
Return Predicted Price: The function returns the actual predicted selling price (ac_y_pred).

st.set_page_config(layout="wide")
Set Page Configuration: This sets the layout of the Streamlit app to "wide", allowing more horizontal space for content.

st.title(":blue[**INDUSTRIAL COPPER MODELING**]")
App Title: This sets the title of the app, "INDUSTRIAL COPPER MODELING", with the text color set to blue.

with st.sidebar:
    option = option_menu('Rohan', options=["PREDICT SELLING PRICE", "PREDICT STATUS"])
Sidebar with Option Menu: This creates a sidebar menu with the title "Rohan" and options for "PREDICT SELLING PRICE" and "PREDICT STATUS". The selected option is stored in the variable option.

if option == "PREDICT STATUS":

    st.header("PREDICT STATUS (Won / Lose)")
    st.write(" ")

    col1, col2 = st.columns(2)
Predict Status Section: If the selected option is "PREDICT STATUS", this code creates a header "PREDICT STATUS (Won / Lose)" and two columns for input fields.

    with col1:
        country = st.number_input(label="**Enter the Value for COUNTRY**/ Min:25.0, Max:113.0")
        item_type = st.number_input(label="**Enter the Value for ITEM TYPE**/ Min:0.0, Max:6.0")
        application = st.number_input(label="**Enter the Value for APPLICATION**/ Min:2.0, Max:87.5")
        width = st.number_input(label="**Enter the Value for WIDTH**/ Min:700.0, Max:1980.0")
        product_ref = st.number_input(label="**Enter the Value for PRODUCT_REF**/ Min:611728, Max:1722207579")
        quantity_tons_log = st.number_input(label="**Enter the Value for QUANTITY_TONS (Log Value)**/ Min:-0.322, Max:6.924", format="%0.15f")
        customer_log = st.number_input(label="**Enter the Value for CUSTOMER (Log Value)**/ Min:17.21910, Max:17.23015", format="%0.15f")
        thickness_log = st.number_input(label="**Enter the Value for THICKNESS (Log Value)**/ Min:-1.71479, Max:3.28154", format="%0.15f")
Column 1: This block of code contains the input fields for various parameters needed to predict the status. Users can enter numerical values within the specified ranges for country, item type, application, width, product reference, quantity tons (log value), customer (log value), and thickness (log value).

    with col2:
        selling_price_log = st.number_input(label="**Enter the Value for SELLING PRICE (Log Value)**/ Min:5.97503, Max:7.39036", format="%0.15f")
        item_date_day = st.selectbox("**Select the Day for ITEM DATE**", ("1", "2", "3", "4", "5", "6", "7", "8", "9", "10", "11", "12", "13", "14", "15", "16", "17", "18", "19", "20", "21", "22", "23", "24", "25", "26", "27", "28", "29", "30", "31"))
        item_date_month = st.selectbox("**Select the Month for ITEM DATE**", ("1", "2", "3", "4", "5", "6", "7", "8", "9", "10", "11", "12"))
        item_date_year = st.selectbox("**Select the Year for ITEM DATE**", ("2020", "2021"))
        delivery_date_day = st.selectbox("**Select the Day for DELIVERY DATE**", ("1", "2", "3", "4", "5", "6", "7", "8", "9", "10", "11", "12", "13", "14", "15", "16", "17", "18", "19", "20", "21", "22", "23", "24", "25", "26", "27", "28", "29", "30", "31"))
        delivery_date_month = st.selectbox("**Select the Month for DELIVERY DATE**", ("1", "2", "3", "4", "5", "6", "7", "8", "9", "10", "11", "12"))
        delivery_date_year = st.selectbox("**Select the Year for DELIVERY DATE**", ("2020", "2021", "2022"))
Column 2: This block of code contains input fields for additional parameters such as selling price (log value) and the item and delivery dates. Users can select the day, month, and year from dropdown menus.

button = st.button(":violet[***PREDICT THE STATUS***]", use_container_width=True)
Predict Status Button: Creates a button with the label "PREDICT THE STATUS". The button uses the full container width and has a violet color.

if button:
    status = predict_status(country, item_type, application, width, product_ref, quantity_tons_log,
                            customer_log, thickness_log, selling_price_log, item_date_day,
                            item_date_month, item_date_year, delivery_date_day, delivery_date_month,
                            delivery_date_year)
Button Click Event: When the button is clicked, it triggers the predict_status function, passing in the values from the input fields.

    if status == 1:
        st.write("## :green[**The Status is WON**]")
    else:
        st.write("## :red[**The Status is LOSE**]")
Display Status Result: Based on the prediction result (status), it displays either "The Status is WON" in green or "The Status is LOSE" in red.

if option == "PREDICT SELLING PRICE":
    st.header("**PREDICT SELLING PRICE**")
    st.write(" ")

    col1, col2 = st.columns(2)
Predict Selling Price Section: If the selected option is "PREDICT SELLING PRICE", it creates a header "PREDICT SELLING PRICE" and two columns for input fields.

    with col1:
        country = st.number_input(label="**Enter the Value for COUNTRY**/ Min:25.0, Max:113.0")
        status = st.number_input(label="**Enter the Value for STATUS**/ Min:0.0, Max:8.0")
        item_type = st.number_input(label="**Enter the Value for ITEM TYPE**/ Min:0.0, Max:6.0")
        application = st.number_input(label="**Enter the Value for APPLICATION**/ Min:2.0, Max:87.5")
        width = st.number_input(label="**Enter the Value for WIDTH**/ Min:700.0, Max:1980.0")
        product_ref = st.number_input(label="**Enter the Value for PRODUCT_REF**/ Min:611728, Max:1722207579")
        quantity_tons_log = st.number_input(label="**Enter the Value for QUANTITY_TONS (Log Value)**/ Min:-0.3223343801166147, Max:6.924734324081348", format="%0.15f")
        customer_log = st.number_input(label="**Enter the Value for CUSTOMER (Log Value)**/ Min:17.21910565821408, Max:17.230155364880137", format="%0.15f")
Column 1: Contains input fields for various parameters needed to predict the selling price. Users can enter numerical values within the specified ranges for country, status, item type, application, width, product reference, quantity tons (log value), and customer (log value).

    with col2:
        thickness_log = st.number_input(label="**Enter the Value for THICKNESS (Log Value)**/ Min:-1.7147984280919266, Max:3.281543137578373", format="%0.15f")
        item_date_day = st.selectbox("**Select the Day for ITEM DATE**", ("1", "2", "3", "4", "5", "6", "7", "8", "9", "10", "11", "12", "13", "14", "15", "16", "17", "18", "19", "20", "21", "22", "23", "24", "25", "26", "27", "28", "29", "30", "31"))
        item_date_month = st.selectbox("**Select the Month for ITEM DATE**", ("1", "2", "3", "4", "5", "6", "7", "8", "9", "10", "11", "12"))
        item_date_year = st.selectbox("**Select the Year for ITEM DATE**", ("2020", "2021"))
        delivery_date_day = st.selectbox("**Select the Day for DELIVERY DATE**", ("1", "2", "3", "4", "5", "6", "7", "8", "9", "10", "11", "12", "13", "14", "15", "16", "17", "18", "19", "20", "21", "22", "23", "24", "25", "26", "27", "28", "29", "30", "31"))
        delivery_date_month = st.selectbox("**Select the Month for DELIVERY DATE**", ("1", "2", "3", "4", "5", "6", "7", "8", "9", "10", "11", "12"))
        delivery_date_year = st.selectbox("**Select the Year for DELIVERY DATE**", ("2020", "2021", "2022"))
Column 2: Contains input fields for additional parameters such as thickness (log value) and the item and delivery dates. Users can select the day, month, and year from dropdown menus.

button = st.button(":violet[***PREDICT THE SELLING PRICE***]", use_container_width=True)
Predict Selling Price Button: Creates a button with the label "PREDICT THE SELLING PRICE". The button uses the full container width and has a violet color.

if button:
    price = predict_selling_price(country, status, item_type, application, width, product_ref, quantity_tons_log,
                                  customer_log, thickness_log, item_date_day, item_date_month, item_date_year,
                                  delivery_date_day, delivery_date_month, delivery_date_year)
Button Click Event: When the button is clicked, it triggers the predict_selling_price function, passing in the values from the input fields.

    st.write("## :green[**The Selling Price is :**]", price)
Display Price Result: Displays the predicted selling price in green with the label "The Selling Price is :".



