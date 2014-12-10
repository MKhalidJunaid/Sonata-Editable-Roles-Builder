Soanta-Editable-Roles-Builder
================================

Sonata Admin Override Security Roles Handler
--

In Sonata admin if you wish to override scerurity roles handler you have to override 2 services

 - sonata.user.editable_role_builder
 - sonata.user.form.type.security_roles

And definitions will look like as below


        <services>
            <service id="sonata.user.editable_role_builder" class="Acme\DemoBundle\Security\EditableRolesBuilder">
                <argument type="service" id="security.context" />
                <argument type="service" id="sonata.admin.pool" />
                <argument>%security.role_hierarchy.roles%</argument>
            </service>
            <service id="sonata.user.form.type.security_roles" class="Acme\DemoBundle\Form\Type\SecurityRolesType">
                <tag name="form.type" alias="sonata_security_roles" />
                <argument type="service" id="sonata.user.editable_role_builder" />
            </service>

        </services>

And define your classes in these services for demo code i have user `Acme\DemoBundle`

Now `SecurityRolesType` class is dependendant of Sonata's `EditableRolesBuilder` you have to make it depandent of your `EditableRolesBuilder` Class so in same way override dependency of sonata's `RestoreRolesTransformer` to your class

Also if you wish to override the twig template for roles you can overrride it by coping `form_admin_fields.html.twig` from `vendor\sonata-project\user-bundle\Resources\views` and adding `app\Resources\SonataUserBundle\views\Form` path it will override parent twig file

Last step import your service file in main configuration file that is `config.yml`

        imports:
            - { resource: parameters.yml }
            - { resource: security.yml }
            - { resource: @AcmeDemoBundle/Resources/config/admin.xml }
