# Python-

To accomplish the given task, you can follow these steps to create a Django application using Django Rest Framework:

Create a Django Project and Application:

bash
Copy code
django-admin startproject myproject
cd myproject
python manage.py startapp invoices
Define Models:
In invoices/models.py, define the Invoice and InvoiceDetail models:

python
Copy code
from django.db import models

class Invoice(models.Model):
    date = models.DateField()
    customer_name = models.CharField(max_length=100)

class InvoiceDetail(models.Model):
    invoice = models.ForeignKey(Invoice, on_delete=models.CASCADE, related_name='details')
    description = models.CharField(max_length=255)
    quantity = models.PositiveIntegerField()
    unit_price = models.DecimalField(max_digits=10, decimal_places=2)
    price = models.DecimalField(max_digits=10, decimal_places=2)
Create Serializers:
In invoices/serializers.py, create serializers for the models:

python
Copy code
from rest_framework import serializers
from .models import Invoice, InvoiceDetail

class InvoiceDetailSerializer(serializers.ModelSerializer):
    class Meta:
        model = InvoiceDetail
        fields = '__all__'

class InvoiceSerializer(serializers.ModelSerializer):
    details = InvoiceDetailSerializer(many=True)

    class Meta:
        model = Invoice
        fields = '__all__'
Create Views and APIs:
In invoices/views.py, create views and APIs using Django Rest Framework:

python
Copy code
from rest_framework import viewsets
from .models import Invoice, InvoiceDetail
from .serializers import InvoiceSerializer, InvoiceDetailSerializer

class InvoiceViewSet(viewsets.ModelViewSet):
    queryset = Invoice.objects.all()
    serializer_class = InvoiceSerializer

class InvoiceDetailViewSet(viewsets.ModelViewSet):
    queryset = InvoiceDetail.objects.all()
    serializer_class = InvoiceDetailSerializer
Configure URLs:
In invoices/urls.py, configure the URLs for the APIs:

python
Copy code
from rest_framework.routers import DefaultRouter
from .views import InvoiceViewSet, InvoiceDetailViewSet

router = DefaultRouter()
router.register(r'invoices', InvoiceViewSet)
router.register(r'invoice-details', InvoiceDetailViewSet)

urlpatterns = router.urls
Run Migrations:

Copy code
python manage.py makemigrations
python manage.py migrate
Create Test Cases:
In invoices/tests.py, create test cases to test all API endpoints.

Run the Server:

Copy code
python manage.py runserver
