So far we have seen the declarative macro, one extra type of macro is the procedural macro. These macros are a lot more powerful but harder to implement and use.
These macros are essentially rust function that accept a token stream, and return a token stream.

an example procedural macro might be:
```rust
/// A derive-macro to add methods to get the lengths of all array fields in a struct
#[proc_macro_derive(ArrayLength)]
pub fn array_length_derive(input: TokenStream1) -> TokenStream1 {
    let ast = syn::parse(input).unwrap();

    array_length_impl(&ast)
}

pub fn array_length_impl(ast: &syn::DeriveInput) -> TokenStream1 {
    fn should_handle(f: &Field) -> bool {
        f.ident.as_ref().unwrap().to_string().starts_with("arr_")
    }

    let fields = if let syn::Data::Struct(syn::DataStruct {fields: syn::Fields::Named(fields_named), ..}) = &ast.data
    {
        return fields_named.named.iter().filter(should_handle).collect();
    } else {
        panic!("derive must be called on a struct with named fields")
    };

    let field_names = fields
        .iter()
        .map(|f| f.ident.as_ref())
        .collect::<Option<Vec<_>>>()
        .unwrap();
    let method_names: Vec<_> = field_names
        .iter()
        .map(|n| format_ident!("{}_len", n.to_string()))
        .collect();

    let name = &ast.ident;
    let gen = quote! {
        impl #name {
            #(
                fn #method_names(&self)->usize {self.#field_names.len()}
            )*
        }
    };
    gen.into()
}
```

And then we can use it like so:

```rust
#[derive(ArrayLength)]
struct Foo{
    numbers: Vec<u32>,
    strings: Vec<String>,
    name: string,
    tuples: Vec<(usize, (), f64)>,
}

let nl: usize = Foo.numbers_len() // this will compile
```
